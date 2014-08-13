---
author: jokecamp
comments: true
date: 2014-03-27 04:02:40+00:00
layout: post
slug: setup-up-your-mac-for-net-development
title: Setup up your Mac for .NET Development
desc: "A list of tools and projects to setup and configure your new mac for .NET development."
wordpress_id: 1328
categories:
- code
tags:
- OSX
---

### Making the Switch from Windows to OSX

I'm a windows .NET developer and I recently bought a new laptop. A Macbook Pro retina. Years ago that would not of been practical decision but it is now a growing trend with the help of virtualization.

The decision was not difficult once you commit to the overall price. In the end I wanted a powerful machine  with a great screen that would allow me to develop against Windows, Android and OS X. I also liked the idea of a unix based operating system as my backend.

### Setting up your OSX developer environment

Enough of the boring reasons. Lets get started with how I setup my new environment. Below are the steps I took in more or less this order.

  * Perform OSX System Updates
  * Download and Install [Chrome](https://www.google.com/intl/en/chrome/browser/) and sign into your google account to sync all your bookmarks.
  * Install the [LastPass chrome extension](https://chrome.google.com/webstore/detail/lastpass-free-password-ma/hdokiejnpimakedhajhdlcegeplioahd?hl=en-US) and sign in. You do use a password manager, right?
  * Install [Parallels 9](http://www.parallels.com/downloads/desktop/) - There is a trial period available but the cost is around $80.
  * [Buy and Download Windows 8.1 ISO](http://www.microsoftstore.com/store/msusa/en_US/pdp/Windows-8.1/productID.288401200) - This turned out to be a little tricky. I had to first buy Windows 8.1 from the Microsoft Store and it was a little confused what version to choose. Their directions are not that straight forward for new users. They assume everyone is updating from Win 7 to Win 8. If you are performing a clean install you will want to buy and download Windows 8.1. The purchased download is a 6 MB exe file that helps you download the install files. However you cannot run this helper exe on your Mac. I had to run the exe on my old Windows 7 machine. After a few steps of entering your serial key and downloading the installation files it will ask you if you want to save the installation as an iso. Once you have the iso (which was around 3 GB I think) you can then transfer it to your macbook (usb stick, removable drive or network transfer).
  * Install Windows 8.1 in a new VM via Parallels  - You can then use the iso to create a new VM with Parallels 9. Once you have the ISO you can install windows in about 10-15 minutes.

[![Windows VM Config](http://jokecamp.files.wordpress.com/2014/01/windows-vm-config.png?w=300)](http://jokecamp.files.wordpress.com/2014/01/windows-vm-config.png)
My VM resource settings

[![Paralles VM Installation](http://jokecamp.files.wordpress.com/2014/01/vm-installation.png?w=300)](http://jokecamp.files.wordpress.com/2014/01/vm-installation.png)
Installing Windows 8.1 via Paralells

[![Windows inside mac](http://jokecamp.files.wordpress.com/2014/01/windows-inside-mac.png?w=300)](http://jokecamp.files.wordpress.com/2014/01/windows-inside-mac.png)
We now have Windows inside OSX. I usually run it in full screen in a separate desktop. It does eat through the battery so I usually try to stay plugged in when doing any heavy virtualization.

  * Install XCode via the Apple App Store
  * Install Visual Studio (and ReSharper if you got it) on your Windows vm
  * Install [Xamarin](http://xamarin.com/download) for your C# fix. I installed the starter trail on my Windows VM and Mac system.

### Call in the experts

At this point I needed to find some advice from experienced Mac developers.

If you take away anything from these guides. It would be to install and use [Homebrew](http://brew.sh/). The "missing package manager for OS X."

  * [nicolashery / mac-dev-setup](https://github.com/nicolashery/mac-dev-setup) by Nicolas Hery
  * [NPR - How to Setup Your Mac to Develop News Applications Like We Do](http://blog.apps.npr.org/2013/06/06/how-to-setup-a-developers-environment.html)

A few quick tips:

 - [Alfred](http://www.alfredapp.com/#download) is  good spotlight replacement. After installing you might want to swap the spotlight shortcuts with the Alfred shortcuts.
 - [Spaces/mutliple desktops](http://mattgemmell.com/using-spaces-on-os-x-lion/)
 - Use the multiple spaces (desktops) feature. It is a vast improvement to fighting with windows sizes. Especially if you have limited real estate like the laptop. Use the Control plus left/right keys to quickly navigation to different spaces (desktops).

You can't directly cut and paste files in OSX. You have to move. You can do this with a copy then a special paste with Option+Command+V

## The Hardest Part of Switching was the shortcuts

The hardest part and what still remains the hardest part of switching is switching back and forth from windows to OSX shortcuts. It can be quite painful especially if you switch often from OSX to your Windows VM.

A quick reminder about the cryptic Apple keys.
⌘   Command key
⌃   Control key
⌥   Option key
⇧   Shift Key
⇪   Caps Lock
Fn  Function Key
