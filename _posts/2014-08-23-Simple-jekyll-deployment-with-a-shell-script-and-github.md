---
comments: true
layout: post
title: Simple jekyll deployment with a shell script and GitHub
desc: 'How to deploy your jekyll static site to a remote unix server using ssh, GitHub and a simple shell script.'
categories:
- Code
tags:
- devops
- jekyll
---

I wanted to document and share my process for deploying this very website (jokecamp.com).

There are many ways to automate the [deployment of a jekyll website](http://jekyllrb.com/docs/deployment-methods/). I tried a few methods but settled on a very simple ssh script I can run from my local computer. These steps could be used for any type of code deployment. It is not specific to a jekyll website.

**Quick Summary**

- Execute the `deploy.sh` script on your local computer and enter your password when prompted
- The script will then
  - connect and login to the remote server via ssh
  - git pull the entire project from GitHub into a private directory
  - Copy the contents of the `_site` folder your public website directory

## Specific Setup and Configuration

Prerequisites:

- commit your website to GitHub
 - include the auto-generated `_site` folder
 - for example: <https://github.com/jokecamp/jokecamp.com/>
- a remote unix server that supports ssh (secure shell) access
 - I use [nearlyfreespeech.net](https://www.nearlyfreespeech.net/)
- a local computer that can run shell scripts
 - Mac, Linux or Windows with [cygwin](https://www.cygwin.com/)

Create the following scripts on your local computer. I prefer and would advise not to commit these to your project. I keep my deployment scripts on [dropbox](http://dropbox.com).

Create `deploy.sh`

```
#!/bin/bash

ssh username@your.remote.host.for.ssh.com 'bash -s' < server-script.sh
```

Create `server-script.sh`

```
#!/bin/bash

echo 'getting latest from github'
cd /home/private/jokecamp.com/
git pull -v

echo 'deploying to home/public'
cp -av /home/private/jokecamp.com/_site/. /home/public/

echo '** done. www.jokecamp.com deployed. **'
```

A couple notes about my environment. `/home/private` is a directory that cannot be accessed from the public facing internet. It is safe to keep the entire GitHub project here. The `/home/public/` directory is the root directory serving this website (jokecamp.com).

You will need to do a one time change of permissions on the scripts in order be able to execute them. Run these commands on your local computer

```
chmod 755 deploy.sh
chmod 755 server-script.sh
```

Now I can deployment the site with **one script**. It will **prompt me for my password** then connect to the site via ssh, pull the latest from GitHub and update my website.

```
$ ./deploy.sh
username@your.remote.host.for.ssh.com's password:
```

That is all. Our website has been deployed!
