---
author: jokecamp
comments: true
date: 2013-06-30 21:20:25+00:00
layout: post
slug: servicestack-api-with-fastcgi-mono-server-and-nginx-hosted-on-digitalocean
title: 'Tutorial: ServiceStack API with FastCGI Mono Server and Nginx hosted on DigitalOcean'
desc: 'A tutorial on running a ServiceStack API with FastCGI Mono Server and Nginx hosted on a DigitalOcean droplet (vm)'
wordpress_id: 927
categories:
- Code
tags:
- api
- asp.net
- fastcgi
- mono
- nginx
- servicestack
featured: true
---

**Who's this targeted for?**

I created this post for my fellow Windows .NET developers who don't know where to start. As a Windows and .NET developer it was difficult for me to get this working. Mostly because of my lack of experience with Linux and the Nginx/FastCGI stack. After going through this install/deploy/config process many times (really an embarrassing number of times) I've come up with a working pattern and documented the process. Hopefully it spares you some of the frustration I encountered. Let me know your thoughts/problems. As a side note, I'm aware I don't follow some best practices. For example, I'm going to use "root" user everywhere!

First some basic information for the uninitiated.

  * [Nginx](http://nginx.com/) is an open source high-performance HTTP server. Pronounce "engine x" for the sake of your street credit.
  * [FastCGI](http://en.wikipedia.org/wiki/FastCGI) is a protocol for interfacing external applications to web servers
  * [Mono](http://www.mono-project.com/) - cross platform, open source .NET development framework. This is how it will our c# code will work on Linux.
  * [FastCGI Mono Server](http://www.mono-project.com/FastCGI_Nginx) - FastCGI server implemented for mono. We will be using fastcgi-mono-server4 for .NET 4.0

### 1) DigitalOcean Server Setup


If you already have a server or a local environment with Ubuntu 12.04 x32. Great. You can skip the to the good stuff.

For this demo I created an account on [DigitalOcean.com](https://www.digitalocean.com/?refcode=831c045735c8). They are a simple cloud hosting site for Linux servers. Its pretty cheap for development work and I've only used it for experiments myself. If you are worried about costs then whenever you are done with any development work then take a snapshot of your server, shut it down and destroy it. **Offline servers still accrue charges but destroyed servers do not**. It is very simple to restore a server from a snapshot so you can preserve your work. Doing this will allow you to work on a server for a few hours and only incur a charge of about a nickel.

I usually create a droplet with 512MB and 1 CPU. This demo was created on the Linux distribution **Ubuntu 12.04 x32.**

[![ImageProperties](http://jokecamp.files.wordpress.com/2013/06/imageproperties.png?w=300)](http://jokecamp.files.wordpress.com/2013/06/imageproperties.png)
<caption align="bottom">DigitalOcean - Create Droplet UI</caption>



### 2) Connect to your Server

You will need a SSH client. For windows I use [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) Your IP address, and root password will be in an email sent from Digital Ocean upon creation of your Droplet. You can change your password with the "passwd" unix command.

Open up PuTTY and connect to the ip as the user "root". Don't worry about the security warning. You will see this often if you are constantly creating/destroying your server.

### 3) Nginx


First install nginx with the following command from any directory

`apt-get install nginx`

and now we are almost done with nginx. Seriously, we just need to start the service now. Start it up with the following command for any directory

`service nginx restart`

Your nginx website is now online. If you hit your ip address from your browser you will see a "Welcome to nginx" page.

[![Picture Of browser and PuTTY terminal](http://jokecamp.files.wordpress.com/2013/06/welcometonginx.png?w=300)](http://jokecamp.files.wordpress.com/2013/06/welcometonginx.png)
<caption>Chrome and PuTTY</caption>

By default the nginx configuration file is located at: `/etc/nginx/sites-available/default` but you don’t need to make any changes to see it working. Looking at the config file you will see that the welcome page is located at `/usr/share/nginx/www/index.html`. We will change this path later. Go ahead and make some html changes to index.html now and you will see your changes reflected after refreshing your browser. No restart of the service is necessary. Just like our old friend IIS.


### 4) Mono FastCGI

You will need to install the following packages:

  1. mono-complete - a large package of the complete Mono runtime, development tools and all libraries.
  2. mono-fastcgi-server4 - the ASP.NET 4.0 backend for FastCGI web servers. It includes mono-xsp4-base

So run the following

```
apt-get install mono-complete
```

then

```
apt-get install mono-fastcgi-server4
```

If you hit your ip again you will notice our "Welcome to nginx" page is still working because we haven't re-configured nginx.

### 5) Deploy your ServiceStack API ASP.NET Website

You are going to need an existing demo API project. You can use one of the official examples but I used a bare bones API for my own purposes. I created an empty WebForms project. Added ServiceStack via nuget then just created a couple routes at the base directory. If you use /api that is great but keep that in mind when testing and configuring your site. If you are having problems you might want to try doing a simple "Hello World" ASP.NET website first. After the project is working locally in Visual Studio I publish to a new folder and will only deploy those files.

Another project you can test with is my open source ServiceStack API project called [Open Football Api at GitHub](https://github.com/jokecamp/OpenFootballApi). It is a little more involved and also includes a mono SQLite dependency. This project also works with the steps in this demo.

**ASP.NET Project Requirements**

  1. First check out the [Mono compatibility](http://www.mono-project.com/Compatibility) list here for a general overview.
  2. You should be **targeting .NET 4.0** or lower
  3. Use MVC 3 or lower or use WebForms. At the time of this writing MVC 4 is only partial implemented.

I used [WinSCP](http://winscp.net/eng/index.php) for this but there are many options. Open up the WinSCP and connect to your host via IP and login with root or any user your may have created.

First, setup your server with some paths we will need. I'm going to use the `/var/www/` directory to hold all my websites.

```mkdir /var/www/```

and lets create a place to eventually put our logs. The p switch will create any required parent directories.

```mkdir --p /var/log/mono/```

Copy the entire fold of your ASP.NET project into `/var/www/`. I had a project called demo that I copied over. After this step I have the following path `/var/www/demo`


### 6) Setup FastCGI

Now we need to tell nginx to proxy requests to mono-fastcgi-server4. We are going to use TCP instead of unix sockets. To do this we need to edit the nginx config. Run the following command:

```nano /etc/nginx/sites-available/default```

Go ahead and delete everything in the file and then add the code below. I've stripped out everything not needed to clarify the code. There are lot of nginx configuration details that are outside the scope of this demo.


```
server {
   location / {
     fastcgi_index Index.aspx;
     fastcgi_pass 127.0.0.1:9000;
     include /etc/nginx/fastcgi_params;
  }
}
```

You should have noticed we are including a `fastcgi_params` file at the bottom. We need to edit that file. Run

```nano /etc/nginx/fastcgi_params```

Add the following two lines to the top

```
fastcgi_param  PATH_INFO          "";
fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
```

Comment out already existing line by adding a #

```
#fastcgi_param  SCRIPT_FILENAME         $request_filename;
```

### 7) Start Mono FastCGI Server

First verify your configuration changes and reload the service with the following combined commands:

```nginx -t && service nginx reload```

If you were to hit your ip now from the browser you would see a “502 Bad Gateway” error. This error could be from a lot of things but it will always get thrown when your mono fastcgi server is not running.

So lets start the mono server by running this command

```
fastcgi-mono-server4 /applications=/:/var/www/demo /socket=tcp:127.0.0.1:9000 /logfile=/var/log/mono/fastcgi.log /printlog=True
```

Add a space and ampersand (&) to the end of the command to exec as a [unix background process](http://kb.iu.edu/data/afnz.html).

You will want to run it as one command but here it is for those with small screens.

```
fastcgi-mono-server4
  /applications=/:/var/www/demo
  /socket=tcp:127.0.0.1:9000
  /logfile=/var/log/mono/fastcgi.log
  /printlog=True
```


You should notice we are using the log path we created earlier with the logfile parameter. We are also going to print any messages to the console/terminal with the `printlog=True` parameter. Also, if we had not specified the application path then we would of been required to run this command from the directory of our website. In this case from `/var/www/demo`. Ok. the console should be running the server now. You should not of seen any messages.

Refresh your browser and you should see your demo application running. If you see a 403 Forbidden error "**do not panic**". Depending on your project you might try `/hello` or `/api/hello`.

[![ServiceStack API Example](http://jokecamp.files.wordpress.com/2013/06/api-example.png?w=300)](http://jokecamp.files.wordpress.com/2013/06/api-example.png)
<caption>ServiceStack API Example</caption>

With every request, even successful requests I see the below message logged in the console. I do not know the source of this but the API should still be working fine and returning results.
`Error   Failed to process connection. Reason: The object was used after being disposed.`

To stop the server you can use `Control+C`

Thanks to these resources!


  * [Nginx for Developers: An Introduction](http://carrot.is/coding/nginx_introduction)
  * [Nginx configuration primer](http://blog.martinfjordvald.com/2010/07/nginx-primer/)
  * [FastCGI Overview](http://www.mono-project.com/FastCGI)
  * [Mono FastCGI and Nginx](http://www.mono-project.com/FastCGI_Nginx)
  * [StackOverflow - Best way to run ServiceStack on Linux and Mono](http://stackoverflow.com/q/12188356/215502)
  * [Github - Run ServiceStack in FastCGI hosted on Nginx](https://github.com/ServiceStack/ServiceStack/wiki/Run-ServiceStack-in-Fastcgi-hosted-on-nginx)
  * [Download example nginx.conf and mono_fastcgi config files shared by @mythz](https://groups.google.com/forum/#!msg/servicestack/kzfS88RldIU/Hz7asC4nA78J)
  * [Ubuntu - Mono Complete Package details](http://packages.ubuntu.com/raring/mono-complete)
  * [Ubuntu - mono-fastcgi-server4 package details](http://packages.ubuntu.com/raring/mono-fastcgi-server4)
  * [Serving your ASP.NET MVC site on Nginx / fastcgi-mono-server4](http://sourcecodebean.com/archives/serving-your-asp-net-mvc-site-on-nginx-fastcgi-mono-server4/1617)
  * [HOWTO: nginx and mono (asp.net)](http://nyanichan.wordpress.com/2011/05/10/howto-nginx-and-mono-asp-net/)
  * [FastCGI Whitepaper](http://www.fastcgi.com/devkit/doc/fastcgi-whitepaper/fastcgi.htm)
  * [Unix File Types](http://en.wikipedia.org/wiki/Unix_file_types)
  * [Unix Permissions](http://www.acm.uiuc.edu/webmonkeys/html_workshop/unix.html)
