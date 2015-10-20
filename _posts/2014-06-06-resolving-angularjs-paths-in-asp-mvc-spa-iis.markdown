---
author: jokecamp
comments: true
date: 2014-06-06 23:49:35+00:00
layout: post
slug: resolving-angularjs-paths-in-asp-mvc-spa-iis
title: Resolving AngularJS paths in a ASP.NET MVC SPA and IIS
desc: "How to resolve routing problems in the development of a AngularJS SPA (single page application) inside an ASP.NET MVC application"
wordpress_id: 1476
categories:
- Code
tags:
- angularjs
- C#
- iis
- javascript
- mvc
- razor
---

<img src="http://jokecamp.files.wordpress.com/2014/06/iis-application.png">
<caption align="right">Problems when your IIS Application is not at the root</caption>

Routing in the development of a AngularJS SPA (single page application) inside an ASP.NET MVC application can be problematic. Trying to resolve relative file paths when developing locally vs deploying to a remote server can have different behaviors, especially when your deployed application does not live at the root of your IIS website. Below is the problem I encountered and my solution.

## The Problem with Relative URLs

For example you may have the following navigation links inside an MVC cshtml file.

```html
<ul>
	<li><a href="/home">Home</a></li>
	<li><a href="/pages/1">Page One</a></li>
	<li><a href="/admins">Admins</a></li>
</ul>
```

Then if you define your AngularJS routes with the following

```javascript
$routeProvider.when('/home',
  { templateUrl: '/angularApp/home/home.html', controller: 'homeCtrl' });

$routeProvider.when('/pages/:id',
  { templateUrl: '/angularApp/pages/pages.html', controller: 'pagesCtrl' });

$routeProvider.when('/admins',
  { templateUrl: '/angularApp/admins/admins.html', controller: 'adminCtrl' });
```

If you deploy this to IIS as a new application (named "app" for instance) instead of under the root of the website then you will encounter the following issues:

  * **The navigation links will not work on the deployed server**. The links will end up point to http://server/home instead of the correct http://server/app/home.
  * **The templateUrl will not resolve correctly inside AngularJS**. AngularJS will look for “http://server//angularApp/admins/admins.html instead of the correct location of http://server/app/angularApp/admins/admins.html

[![My AngularJS SPA Visual Studio Project Structure](http://jokecamp.files.wordpress.com/2014/06/directory-structure.png?w=85)](https://jokecamp.files.wordpress.com/2014/06/directory-structure.png)
<caption>My AngularJS SPA Visual Studio Project Structure</caption>

## Solution

There are probably a few different solutions but I will outline the one that worked for me and allowed to work locally and deploy without any configuration changes. To avoid configuration changes we can use a couple tricks involving the an HTML [base tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base) and a [C# conditional compilation directive](http://msdn.microsoft.com/en-us/library/aa691099(v=vs.71).aspx). For this to work we have to assume we will always compile in DEBUG for localhost and compile in RELEASE for remote deployment.

  * Create a base html tag and conditionally set the href value based DEBUG vs RELEASE. If in DEBUG then use href = '/' otherwise use href = '/app/'
  * Change all html links to be relative paths therefore utilizing the base url
  * In AngularJS change the $routeProvider templateUrls to prepend the base url

Here is the conditional code in the Razor file ( _Layout.cshtml)

```html
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="description" content="">
<title>@ViewBag.Title</title>
    @Styles.Render("~/bundles/css")

    @if (!Html.IsDebug()) {
       <base href="/app/" />
    } else {
       <base href="/" />
    }
</head>
```

Here is the AngularJS routing code. It uses jQuery to find the first base tag and retrieve the href value

```javascript
var baseUrl = $("base").first().attr("href");
console.log("base url for relative links = " + baseUrl);

$routeProvider.when('/home',
  { templateUrl: baseUrl + 'angularApp/home/home.html', controller: 'homeCtrl' });

$routeProvider.when('/admins',
  { templateUrl: baseUrl + 'angularApp/admins/admins.html', controller: 'adminsCtrl' });

$routeProvider.when('/pages/:id',
  { templateUrl: baseUrl + 'angularApp/endpoints/pages.html', controller: 'pagesCtrl' });
```

Here is my BundleConfig.cs for reference. We can still use virtual paths.

```csharp
bundles.Add(new Bundle("~/bundles/app-scripts")
  .Include("~/angularApp/app.js")

  // Modules/Components
  .Include("~/angularApp/admins/*.js")
  .Include("~/angularApp/home/*.js")
  .Include("~/angularApp/pages/*.js")
);
```

I arrived at this solution thanks to the answers to these SO questions

  * [Using a Relative Path for a Service Call in AngularJS](http://stackoverflow.com/a/17013430/215502) this answer has better guidance on properly injecting the base url for the routes. I have simplified my example and use the base tag instead of the link tag.
  * [Razor view engine, how to enter preprocessor #if debug](http://stackoverflow.com/a/7135343/215502)
