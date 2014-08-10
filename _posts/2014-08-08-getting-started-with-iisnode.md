---
author: jokecamp
comments: true
date: 2014-08-08
layout: post
slug: 'Getting started with iisnode: node.js on Windows'
title: 'Getting started with iisnode: node.js on Windows'
categories:
- guide
tags:
- iisnode
- iis
- nodejs
- windows
---

I've spent some time learning and working with [Tomasz Janczuk's](https://twitter.com/tjanczuk) impressive [iisnode](https://github.com/tjanczuk/iisnode) project. He has done well to explain the open source project but the documentation is fragmented between the [source code](https://github.com/tjanczuk/iisnode/tree/master/src/iisnode), [github issues](https://github.com/tjanczuk/iisnode/issues) and [tjanczuk's blog](http://tomasz.janczuk.org/). I will retrace my learning process in order to help others get started with iisnode.

## Why use issnode?

Here are the three main benefits taken directly from the [project wiki](https://github.com/tjanczuk/iisnode/wiki#benefits)

> **Process management**. The iisnode module takes care of lifetime management of node.exe processes making it simple to improve overall reliability. You don’t have to implement infrastructure to start, stop, and monitor the processes.

> **Side by side with other content types**. The iisnode module integrates with IIS in a way that allows a single web site to contain a variety of content types. For example, a single site can contain a node.js application, static HTML and JavaScript files, PHP applications, and ASP.NET applications. This enables choosing the best tools for the job at hand as well progressive migration of existing applications.

> **Scalability on multi-core servers**. Since node.exe is a single threaded process, it only scales to one CPU core. The iisnode module allows creation of multiple node.exe processes per application and load balances the HTTP traffic between them, therefore enabling full utilization of a server’s CPU capacity without requiring additional infrastructure code from an application developer.

> [See the full list...](https://github.com/tjanczuk/iisnode/wiki#benefits)

Some other notable benefits:

- Use built-in IIS features like security. You could put your nodejs server behind windows authentication
- Use your already existing windows technology stack and avoid complicating the deployment process
- Take advantage of the <span title='Would you save I have a plethora of pinatas?'>plethora<span> of node packages available
- No unix servers or [unix administrators](http://xkcd.com/149/ "The ones who go around demanding sandwhiches") necessary

#### What are the alternatives to iisnode for Windows?

- Run nodejs as a windows service with [node-windows](https://github.com/coreybutler/node-windows)
- Run nodejs as a windows service with [NSSM - the Non-Sucking Service Manager](http://nssm.cc/)
- Run nodejs on a unix sever
- Run it in a windows console and cross your fingers that no one closes the window... ever... Just kidding. Although it is fun to pretend it is this easy.

## Ok, I'll try it. What is iisnode?

[iisnode](https://github.com/tjanczuk/iisnode) is an open source **native IIS module** written in C++ that allows [node.js](http://nodejs.org/) (node.exe) to be run inside Windows IIS.

First, a quick reminder about the difference between managed vs native modules.

 > A module is either a Win32 DLL (native module) or a .NET 2.0 type contained within an assembly (managed module)" <br>-Full article at [iis.net](http://www.iis.net/learn/get-started/introduction-to-iis/iis-modules-overview)

To reiterate, iisnode is a native module. The C++ code gets compiled into a `iisnode.dll` file and installed into IIS as a module. Download and install iisnode via the [releases page](https://github.com/tjanczuk/iisnode/releases/). Be sure to select the MSI that matches your processor (x86 vs x64).

### How does iisnode work?

To over simplify here is how it works with the assumption that we have a nodejs server called `sever.js`

1. A client browser makes a request to your server.js in IIS

2. From your web.config IIS sees that you want to use iisnode module for `server.js` requests

    ```<add name="iisnode" path="hello.js" verb="*" modules="iisnode" />```

3. iisnode will create a node.exe process (or use existing) and communicate through a named pipe

4. node.exe sends the response back through the named pipe, iisnode and then iis

<a href='http://tomasz.janczuk.org/2011/10/architecture-of-iisnode.html'>
<img style="float: right; margin:10px;" src="/public/iisnode-diagram.png" title='See Architecture Diagram' />
</a>

For a more complete picture see the diagrams on [Top Benefits of Running Node.js on Windows Azure](http://blogs.msdn.com/b/hanuk/archive/2012/05/05/top-benefits-of-running-node-js-on-windows-azure.aspx). The article is from the context of Azure but it gives the best description of what is happening under the hood. Other recommended reading (with diagrams) includes [Introduction to IIS Architectures](http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#HTTP) and [Difference between URL Rewriting and URL Routing](http://www.planetofcoders.com/difference-url-rewriting-url-routing/).

If you want to explore the iisnode C++ source code I would start with this blog post
**[Architecture of iisnode](http://tomasz.janczuk.org/2011/10/architecture-of-iisnode.html)** by the creator.
The post describes the key components, relationships and a short description of the most important classes serving as a map of browsing the actual source code on github. **This post was instrumental in understanding how iisnode works**.

## iisnode key points and lessons learned

Some key points to remember when working with iisnode. Reading though these will save you time.


 - Your nodejs server must listen on the `process.env.PORT` variable passed from iisnode. This will end up being the named pipe instead of the traditional port number.

 ```js
 http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('Hello, world! [configuration sample]');
}).listen(process.env.PORT);
```

-  You can promote server variables from IIS into nodejs. If you use windows authentication you can promote the username. See this [github issue](https://github.com/tjanczuk/iisnode/issues/87#issuecomment-3452395).

    Use can specify which variables to promote with a csv value like below.

```xml
<iisnode promoteServerVars="AUTH_USER,AUTH_TYPE" />
```

These values get added to incoming requests as headers. In your nodejs code you then reference the values like below.

```js
var username = req.headers['x-iisnode-auth_user']; // for example AD\username
var authenticationType = req.headers['x-iisnode-auth_type'];
```

-  You can use the URL Rewrite (native) module choose which requests get handled by iisnode and which requests skips iisnode. This was required when setting up a socket.io server inside iisnode.

 ```xml
 <rewrite>
  <rules>
    <rule name="myapp">
      <match url="myapp/*" />
      <action type="Rewrite" url="server.js" />
    </rule>
  </rules>
</rewrite>
```

 - iisnode can act as a load balancer. One IIS app can spawn up several node.exe processes.
  - this is configured in the web.config with `nodeProcessCountPerApplication=2`
  - the balancing is round robin load balancing. Which just means taking turns sequential.
  - See [Scalability on multi-core servers](http://tomasz.janczuk.org/2011/08/hosting-nodejs-applications-in-iis-on.html) for more on load balancing

 - Several independent node.js applications can be present within a single IIS web application. You could have `server1.js`, `server2.js` and ect...

 - Getting iisnode to work with IIS Express can be challenging depending on your Visual Studio version and processor. It is worthy of a separate blog post. Often I run my nodejs servers locally with `nodemon` for development work then deploy to remote windows server with IIS for QA.

## iisnode configuration

There are multiple ways to configure iisnode. Configuration will take this precedence order (1 taking precedence over 2 and ect)

  1. yaml file. See example [iisnode.yml](https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/iisnode.yml)

  2. environment variables. i.e. `process.env.PORT`

  3. web.config. View example [web.config](https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config)

  4. defaults to values in [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml "See default values")

      Here are the defaults to give you an idea of what options are available.

```xml
<attribute name="node_env" type="string" expanded="true" defaultValue="%node_env%"/>
<attribute name="asyncCompletionThreadCount" type="uint" defaultValue="0"/>
<attribute name="nodeProcessCountPerApplication" type="uint" defaultValue="1"/>
<attribute name="nodeProcessCommandLine" type="string" expanded="true" defaultValue="node.exe"/>
<attribute name="interceptor" type="string" expanded="true"
    defaultValue="&quot;%programfiles%\iisnode\interceptor.js&quot;" />
<attribute name="maxConcurrentRequestsPerProcess" type="uint" allowInfitnite="true" defaultValue="1024"/>
<attribute name="maxNamedPipeConnectionRetry" type="uint" defaultValue="100"/>
<attribute name="namedPipeConnectionRetryDelay" type="uint" defaultValue="250"/>
<attribute name="maxNamedPipeConnectionPoolSize" type="uint" defaultValue="512"/>
<attribute name="maxNamedPipePooledConnectionAge" type="uint" defaultValue="30000"/>
<attribute name="initialRequestBufferSize" type="uint" defaultValue="4096"/>
<attribute name="maxRequestBufferSize" type="uint" defaultValue="65536"/>
<attribute name="uncFileChangesPollingInterval" type="uint" defaultValue="5000"/>
<attribute name="gracefulShutdownTimeout" type="uint" defaultValue="60000"/>
<attribute name="logDirectory" type="string" expanded="true" defaultValue="iisnode"/>
<attribute name="debuggingEnabled" type="bool" defaultValue="true"/>
<attribute name="debuggerExtensionDll" type="string" defaultValue="iisnode-inspector-0.7.3.dll"/>
<attribute name="debugHeaderEnabled" type="bool" defaultValue="false"/>
<attribute name="debuggerVirtualDir" type="string" defaultValue="" />
<attribute name="debuggerPathSegment" type="string" expanded="true" defaultValue="debug"/>
<attribute name="debuggerPortRange" type="string" expanded="true" defaultValue="5058-6058"/>
<attribute name="maxLogFileSizeInKB" type="uint" defaultValue="128"/>
<attribute name="maxTotalLogFileSizeInKB" type="uint" defaultValue="1024"/>
<attribute name="maxLogFiles" type="uint" defaultValue="20"/>
<attribute name="loggingEnabled" type="bool" defaultValue="true"/>
<attribute name="devErrorsEnabled" type="bool" defaultValue="true"/>
<attribute name="flushResponse" type="bool" defaultValue="false"/>
<attribute name="watchedFiles" type="string" expanded="true" defaultValue="*.js;iisnode.yml"/>
<attribute name="enableXFF" type="bool" defaultValue="false"/>
<attribute name="promoteServerVars" type="string" defaultValue=""/>
<attribute name="configOverrides" type="string" expanded="true" defaultValue="iisnode.yml"/>
<attribute name="recycleSignalEnabled" type="bool" defaultValue="false"/>
```

### Brief notes on iisnode and socket.io

- if you are trying to setup a socket server keep the `nodeProcessCountPerApplication=1` setting at 1. The socket handshake will fail if the client ends up negotiating with multiple/different node.exe instances.

- Most of the example code and demos assume you will host your application at the "Default Web Site" root level in iis. However, if you are hosting at a sub diretory level like "Default Web Site/app." You will need to add some extra configuration for the socket.io server. You will need to specify the full paths for socket.io.

- socket.io has just upgraded its api from 0.9 to 1.0. Most of the iisnode examples target v 0.9. Be mindful of your version since there were some socket.io API changes.

- a good test is trying to to see if iisnode can successfully load the client library at `fullpath/socket.io/socket.io.js`


### Troubleshooting

- Check the `nodeProcessCommandLine` parameter in web.config. It specifies the path to node.exe installation. Does the path/file exist?
- Check the `interceptor` parameter in the web.config. Does the path/file exist?

### Resources

- [Hosting node.js applications in IIS on Windows](http://tomasz.janczuk.org/2011/08/hosting-nodejs-applications-in-iis-on.html) by Tomas Janczuk
- [Hosting express node.js applications in IIS using iisnode](http://tomasz.janczuk.org/2011/08/hosting-express-nodejs-applications-in.html) by Tomas Janczuk
- [Developing node.js applications in WebMatrix and testing in IIS Express on Windows](http://tomasz.janczuk.org/2011/08/developing-nodejs-applications-in.html) by Tomas Janczuk
- [Installing and Running node.js applications within IIS on Windows - Are you mad?](http://www.hanselman.com/blog/InstallingAndRunningNodejsApplicationsWithinIISOnWindowsAreYouMad.aspx) by Scott Hanselman
- [Debugging IISNODE Applications: new node-inspector integrated with iisnode](http://ranjithblogs.azurewebsites.net/?p=98) by Ranjith Ramachandra
