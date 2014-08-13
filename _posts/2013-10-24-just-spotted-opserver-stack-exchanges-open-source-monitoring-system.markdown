---
author: jokecamp
comments: true
date: 2013-10-24 22:45:08+00:00
layout: post
slug: just-spotted-opserver-stack-exchanges-open-source-monitoring-system
title: Just spotted Opserver - Stack Exchange's open source .NET monitoring system
desc: "Learn about Opserver, the Stack Exchange open source .NET monitoring and devops website."
wordpress_id: 1107
categories:
- Code
tags:
- .NET
- C#
- devops
- open-source
---

As a .NET developer were are used to finding ourselves left out in the cold when it comes to awesome open source projects. However, over the last few years there has been a lot of momentum gaining around C# to reverse that trend. We now have awesome things like the [Mono project](http://www.mono-project.com/Main_Page), [scriptcs](http://scriptcs.net/), [ServiceStack](http://www.servicestack.net/), [RestSharp](https://github.com/restsharp/RestSharp) and everyone's favorite friend [NuGet](http://www.nuget.org/)!

Periodically, I like to check out the [GitHub trending repositories for C#](https://github.com/trending?l=csharp&since=weekly). It is great to see what projects are most popular in the open source community for any given language.

The current trending list for the week of 10/24/13 is below.

  1. [opserver/Opserver](https://github.com/opserver/Opserver) C# -Stack Exchange's Monitoring System
  2. [ServiceStack/ServiceStack](https://github.com/ServiceStack/ServiceStack) C# - Thoughtfully architected, obscenely fast, thoroughly enjoyable web services for all
  3. [SignalR/SignalR](https://github.com/SignalR/SignalR) C# - Incredibly simple real-time web for .NET
  4. [restsharp/RestSharp](https://github.com/restsharp/RestSharp) C# - Simple REST and HTTP API Client for .NET
  5. [Squirrel/Squirrel.Windows](https://github.com/Squirrel/Squirrel.Windows) C# - It's like ClickOnce but Works
  6. [proletariatgames/CUDLR](https://github.com/proletariatgames/CUDLR) C# - Console for Unity Debugging and Logging Remotely

**It is an exciting time to be a C# .NET developer.**

At the top of that list is a new project called **Opserver** that has recently been added by [StackExchange](http://stackexchange.com/). StackExchange, the creators of [StackOverflow](http://stackoverflow.com/) are strong proponents of open-source projects. While they will [not be open sourcing there Q&A engine](http://meta.stackoverflow.com/questions/3086/will-open-sourcing-stack-overflow-destroy-our-business-model) they have used and contributed to many other open source projects. As a C# .NET developer I have always taken solace and pride that the StackOverflow website was a product of C# .NET. Among the horde of unpolished enterprise applications and government websites it was always nice to point to StackOverflow as a great example.

I've been excited ever since the [announcement ](http://blog.stackoverflow.com/2012/02/stack-exchange-open-source-projects/)that StackExchange would be trying to [open source more of their projects](http://nickcraver.com/blog/2012/04/12/what-weve-been-up-to/). Looks like they are getting ready to unleash their server monitoring application. There is a not a lot of documentation yet but it does not take much to get started.

Here are some more [detailed screenshots](http://imgur.com/a/dawwf) posted by Nick Craver.

**The steps I took to get the website running in VisualStudio 2012 **

  * Copied repository to my machine via Windows GitHub
  * Renamed the file Config/SecuritySettings.config.example to SecuritySettings.config
  * Commented out the AD provider and un-commented the following

    ```
    <SecuritySettings provider="alladmin" />
    ```

  * Renamed the file Config/SQLSettings.json.example to SQLSettings.json
  * In SQLSettings I made the clusters an empty string and just added two databases names to the instances list
  * The same for the Config/RedisSettings.json.example file
  * Start debugging the website. You might need to be running as admin and/or have permission to access the servers you listed.

**Screenshots of the Opserver Dashboards for SQL Databases and Redis**

[![all-servers](http://jokecamp.files.wordpress.com/2013/10/all-servers.png?w=300)](http://jokecamp.files.wordpress.com/2013/10/all-servers.png)

[![databases](http://jokecamp.files.wordpress.com/2013/10/databases.png?w=300)](http://jokecamp.files.wordpress.com/2013/10/databases.png)

[![redis](http://jokecamp.files.wordpress.com/2013/10/redis.png?w=300)](http://jokecamp.files.wordpress.com/2013/10/redis.png)

[![top](http://jokecamp.files.wordpress.com/2013/10/top.png?w=300)](http://jokecamp.files.wordpress.com/2013/10/top.png)

As you can see from the screenshots there is a lot of reports and data available and this is only a couple of the many different types of dashboards. **This is a repository to keep your eye on. I can easily see it become the devops dashboard of choice for .NET environments.**
