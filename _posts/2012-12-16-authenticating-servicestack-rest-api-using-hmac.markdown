---
author: jokecamp
comments: true
date: 2012-12-16 15:53:22+00:00
layout: post
slug: authenticating-servicestack-rest-api-using-hmac
title: Authenticating ServiceStack REST API using HMAC
desc: "A code demo to show hot to create a ServiceStack RESTful API with HMAC Security and Authentication"
wordpress_id: 571
categories:
- Code
tags:
- .NET
- api
- C#
- hmac
- rest
- security
- servicestack
- sha256
---

I've created the following C# code to show how HMAC security could be integrated into ServiceStack REST API using the built in [RequestFilter attributes](https://github.com/ServiceStack/ServiceStack/wiki/Filter-attributes). I wanted to try to emulate the security model of [Amazon's S3 authentication](http://s3.amazonaws.com/doc/s3-developer-guide/RESTAuthentication.html). View that page for finer details about the request signature process.

The two main methods are **CreateToken** and **FlattenRequestDetails**. They are responsible for creating the SHA-256 hash and flatting the request details into a [canonical string](http://stackoverflow.com/questions/280107/what-does-the-term-canonical-form-or-canonical-representation-in-java-mean), respectively.

A few notes when integrating with your existing API

  * You need to either decorate your DTOs or Services with the AuthSignatureRequired attribute to secure them
  * Setup a user repository with User Ids and secrets (make sure the secrets are not exposed outside the server)
  * Provide your secret to the consumers ahead of time
  * The consumer's system clock must be within 15 minutes of the server's system clock
  * You won't be able to browse the resources with the metadata pages anymore
  * PUTs, POSTs and DELETES will take a little more code. You could implement an MD5 hash of the content and the ContentType as part of the canonical string
  * The stateless calls work well in environments where you could have multiple servers behind a load balancer

[ServiceStack ](https://github.com/ServiceStack/ServiceStack)already has a great [authentication plugin](https://github.com/ServiceStack/ServiceStack/wiki/Authentication-and-authorization). However, instead of using one of those providers I wanted to experiment with a stateless authentication using [HMAC](http://en.wikipedia.org/wiki/Hash-based_message_authentication_code) SHA-256.

I have only included the relevant code below. To see all the code I've created a [Gist on Github](https://gist.github.com/4302446). The gist includes the AppHost, DTOs, Repos and Services.

**The Implementation**

```csharp
public class ApiCustomHttpHeaders
{
	public static string UserId = "X-CUSTOM-API-USERID";
	public static string Signature = "X-CUSTOM-SIGNATURE";
	public static string Date = "X-CUSTOM-DATE";
}

///
/// The filter will be execute on every request for every DTO or RestService with this Attribute:
///
public class AuthSignatureRequired :
  ServiceStack.ServiceInterface.RequestFilterAttribute, IHasRequestFilter
{
	private static readonly ILog Logger =
    LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);
	public UserRepository UserRepository { get; set; }

	public new int Priority
	{
		// =0 Run after
		get { return -1; }
	}

	private bool CanExecute(IHttpRequest req)
	{
		DateTime requestDate;
		if (!DateTime.TryParse(ApiSignature.GetDate(req), out requestDate))
		{
			throw new SecurityException("You must provide a valid request date in the headers.");
		}

		var difference = requestDate.Subtract(DateTime.Now);
		if (difference.TotalMinutes > 15 || difference.TotalMinutes < -15)
		{
			throw new SecurityException(string.Format(
					"The request timestamp must be within 15 minutes of the server time. Your request is {0} minutes compared to the server. Server time is currently {1} {2}",
					difference.TotalMinutes,
					DateTime.Now.ToLongDateString(),
					DateTime.Now.ToLongTimeString()));
		}

		var userId = ApiSignature.GetUserId(req);
		if (userId <= 0)
		{
			throw new SecurityException("You must provide a valid API User Id with your request");
		}

		var signature = ApiSignature.GetSignature(req);
		if (string.IsNullOrEmpty(signature))
		{
			throw new SecurityException("You must provide a valid request signature (hash)");
		}

		var user = UserRepository.GetById(userId);
		if (user == null || user.Id == 0)
		{
			throw new SecurityException("Your API user id could not be found.");
		}

		if (!user.IsEnabled)
			throw new SecurityException("Your API user account has been disabled.");

		if (signature == ApiSignature.CreateToken(req, user.Secret))
		{
			Logger.InfoFormat("Successfully Authenticated {0}:{1} via signature hash", user.Id, user.Name);
			return true;
		}

		throw new SecurityException("Your request signature (hash) is invalid.");
	}

	public override void Execute(IHttpRequest req, IHttpResponse res, object requestDto)
	{
		var authErrorMessage = "";
		try
		{
			// Perform security check
			if (CanExecute(req))
				return;
		}
		catch (Exception ex)
		{
			authErrorMessage = ex.Message;
			Logger.ErrorFormat("Blocked unauthorized request: {0} {1} by ip = {2} due to {3}",
					req.HttpMethod,
					req.AbsoluteUri,
					req.UserHostAddress ?? "unknown",
					authErrorMessage);
		}

		// Security failed!
		var message = "You are not authorized. " + authErrorMessage;
		//throw new HttpError(HttpStatusCode.Unauthorized, message);

		res.StatusCode = (int)HttpStatusCode.Unauthorized;
		res.StatusDescription = message;
		res.AddHeader(HttpHeaders.WwwAuthenticate, string.Format("{0} realm=\"{1}\"", "", "custom api"));
		res.ContentType = ContentType.PlainText;
		res.Write(message);
		res.Close();
	}
}

/// Static class will perform the flattening of the request and creation
/// of the hash. This is designed to be used by the server and could be distributed
/// as part of an SDK. The Test.aspx example uses this class.
///
public static class ApiSignature
{
	private static readonly ILog Logger =
    LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

        /// Used by SDK and clients to make requests, so we must use the HttpWebRequest class
	public static string CreateToken(HttpWebRequest webRequest, string secret)
	{
		return CreateToken(
		  FlattenRequestDetails(webRequest.Method,
		                        webRequest.RequestUri.AbsoluteUri,
		                        webRequest.ContentType,
		                        webRequest.Date.ToUniversalTime().ToString("r")
		    ), secret);
	}

        /// Used by Server so we must use the Service Stack IHttpRequest
	///
	public static string CreateToken(IHttpRequest request, string secret)
	{
		return CreateToken(
		  FlattenRequestDetails(request.HttpMethod,
		                        request.AbsoluteUri,
		                        request.ContentType,
		                        GetDate(request)
		    ), secret);
	}

	private static string CreateToken(string message, string secret)
	{
		// don't allow null secrets
		secret = secret ?? "";
		var encoding = new System.Text.ASCIIEncoding();
		byte[] keyByte = encoding.GetBytes(secret);
		byte[] messageBytes = encoding.GetBytes(message);
		using (var hmacsha256 = new System.Security.Cryptography.HMACSHA256(keyByte))
		{
			byte[] hashmessage = hmacsha256.ComputeHash(messageBytes);
			return Convert.ToBase64String(hashmessage);
		}
	}

	private static string FlattenRequestDetails(
    string httpMethod, string url, string contentType, string date)
	{
		// If it is a GET then we don't care about the contentType since there will never be contentTypes with GET.
		if (httpMethod.ToUpper() == "GET")
			contentType = "";

		var message = string.Format("{0}{1}{2}{3}", httpMethod, url, contentType, date);
		Logger.Debug("Request message to hash: " + message);
		return message;
	}

        /// If the user is providing the date via the custom header then the server
	/// will use that for the hash. Otherwise we check for the default "Date" header.
	/// This is nessary since some consumers can't control the date header in their web requests
	///
	public static string GetDate(IHttpRequest request)
	{
		return request.Headers[ApiCustomHttpHeaders.Date] ?? request.Headers["Date"] ?? "";
	}

	public static int GetUserId(IHttpRequest req)
	{
		int userId = 0;
		var user = req.Headers[ApiCustomHttpHeaders.UserId] ?? "";
		int.TryParse(user, out userId);
		return userId;
	}

	public static string GetSignature(IHttpRequest req)
	{
		return req.Headers[ApiCustomHttpHeaders.Signature] ?? "";
	}
}
```

**Testing your API with signed Requests (Client/Consumer Code)**

I've included an example of how the consumer's can call your API and sign your request. In this manner you can use the built in ServiceStack Clients. The LocalHttpWebRequestFilter event fires before every request. You only need to wire this up once if you are making multiple calls. Try removing different headers to see how the API responds to invalid requests.

```csharp
try
{
    var client = new JsonServiceClient();

    client.LocalHttpWebRequestFilter +=
        delegate(HttpWebRequest request)
            {
                // ContentType still null at this point so we must hard code it
                // Set these fields before trying to create the token!
                request.ContentType = ServiceStack.Common.Web.ContentType.Json;
                request.Date = DateTime.Now;

                var secret = "5771CC06-B86D-41A6-AB39-9CA2BA338E27";
                var token = ApiSignature.CreateToken(request, secret);
                request.Headers.Add(ApiCustomHttpHeaders.UserId, "1");
                request.Headers.Add(ApiCustomHttpHeaders.Signature, token);
            };

    var teams = client.Get("http://localhost:59833/api/teams");
    foreach (var team in teams)
    {
        Label1.Text += team.Name + "";
    }
}
catch (WebServiceException ex)
{
    Label1.Text = ex.Message + " : " + ex.ResponseBody;
}
```
