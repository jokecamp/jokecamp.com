---
author: jokecamp
comments: true
date: 2012-07-26 13:06:46+00:00
layout: post
slug: how-to-create-an-asp-net-httphandler-to-serve-files-from-local-system
title: How to create an ASP.NET HttpHandler to serve files from local system
desc: "Tutorial on how to create an ASP.NET HttpHandler to serve files from local system with C# code"
wordpress_id: 340
categories:
- Code
tags:
- asp.net
- audio
- C#
- html5
---

**How do you serve files on a local system (or network share) from an ASP.NET website? **

The problem is that browsers do not play well with file:/// as links. IE will allow it but Chrome and Firefox will block these links for security purposes.  See [http://stackoverflow.com/questions/1254572/cross-browser-link-to-file-on-local-system](http://stackoverflow.com/questions/1254572/cross-browser-link-to-file-on-local-system)

One solution is to create an HTTP Handler in your ASP.NET application. This handler will simply retrieve the network file and serve it up.

```csharp
public class Downloader : IHttpHandler
{
    public void ProcessRequest(HttpContext context)
    {
        string path = "//networkshare/path/hey.wav";

        // Hard coded but you can easily get from url parameters
        // string path = context.Request["path"];

        var file = new FileInfo(path);

        // You should check for file.Exists !

        context.Response.Clear();
        context.Response.AddHeader("content-disposition",
            string.Format("attachment; filename=\"{0}\"", file.Name));
        context.Response.ContentType = "audio/wav";
        context.Response.WriteFile(file.FullName, false);
    }

    public bool IsReusable
    {
        get { return false; }
    }
}
```

Now we can allow users to download the file with direct link **Downloader.ashx?path=x**.

**Audio Files**

If you are serving audio files (as my example is doing) you can serve the file with the new HTML5 audio tag. Be sure your Response.ContentType agrees with the audio source type. Support for the different mime types and codecs is very inconsistent. You will want to [earn more about the current state of HTML 5 audio and modern browsers](http://html5doctor.com/html5-audio-the-state-of-play/) before getting too deep in audio.

```html
<audio controls="controls">
  <source src="Downloader.ashx?path=x" type="audio/wav" />
  Your browser does not support the audio element.
</audio>
```

[See if your browser supports the new HTML 5 audio tags](http://jplayer.org/HTML5.Audio.Support/)
