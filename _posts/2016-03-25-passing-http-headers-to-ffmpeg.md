---
author: Joe Kampschmidt
comments: true
layout: post
slug: passing-http-headers-to-ffmpeg
title: Passing HTTP Headers to FFmpeg
desc: 'A very simple unix based example to passing HTTP Headers to the FFmpeg (audio conversion) command line program.'
tags:
- tidbit
---

[FFmpeg][3] is

> a very fast video and audio converter that can also grab from a live audio/video source. It can also convert between arbitrary sample rates and resize video on the fly with a high quality polyphase filter.

Great. So how do you consume a url with custom HTTP headers? The [documentation][0] for providing custom HTTP headers is non existent.

The below example is for adding a single header:

```
ffmpeg -headers $'X-API-KEY: MyApiKey' -i http://localhost:8080/file -v trace

# Results in

GET /file HTTP/1.1
User-Agent: Lavf/56.40.101
Accept: */*
Range: bytes=0-
Connection: close
Host: localhost:8080
Icy-MetaData: 1
X-API-KEY: MyApiKey
```


The below example is for adding multiple headers. Notice the **required line breaks** `\r\n`.

```
ffmpeg -headers $'X-API-KEY: MyApiKey\r\nX-API-SECRET:asfd\r\n' -i http://localhost:8080/file -v trace

# Results in:

GET /file HTTP/1.1
User-Agent: Lavf/56.40.101
Accept: */*
Range: bytes=0-
Connection: close
Host: localhost:8080
Icy-MetaData: 1
X-API-KEY: MyApiKey
X-API-SECRET:asfd
```

If you need to do this from a Windows command line you will have [issues injecting the CRLF][1]. Read more about [potential fixes][2] (and more than you ever wanted to know about line breaks in Windows).

The `-v trace` options gives the most detailed logging.

[0]:http://ffmpeg.org/ffmpeg.html
[1]:https://trac.ffmpeg.org/ticket/3268
[2]:http://thread.gmane.org/gmane.comp.video.ffmpeg.devel/189500
[3]:http://ffmpeg.org/
