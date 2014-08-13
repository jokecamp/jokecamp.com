---
author: jokecamp
comments: true
date: 2014-06-08 01:34:58+00:00
layout: post
slug: building-a-raspberry-pi-photo-frame
title: Building a Raspberry Pi Photo Frame
desc: "Learn how to make a raspberry pi photo frame that downloads and displays your flickr photos"
wordpress_id: 1532
categories:
- blog
tags:
- hobby
- pi
- python
- raspberrypi
- unix
---

First off, my inspiration and instructions came from this article [Building a living photo frame with a Raspberry Pi and a motion detector](http://www.ofbrooklyn.com/2014/01/2/building-photo-frame-raspberry-pi-motion-detector). The author is more knowledgeable and the article is more in-depth than mine. See that article for specific configuration and for the actual contents of the python script to pull photos from flickr. I hope my post will help others by sharing my first time experiences with the pi and the issues/frustrations I encountered.

## The shopping list

Purchased from [Adafruit](http://www.adafruit.com/).

  * 1 x Raspberry Pi Model A 256MB RAM[ID:1344] = $29.95

  * 1 x RCA coupler - Male to Male[ID:951] = $1.50

  * 1 x Miniature WiFi (802.11b/g/n) Module: For Raspberry Pi and more[ID:814] = $11.95

  * 1 x NTSC/PAL (Television) TFT Display - 3.5" Diagonal[ID:913] = $44.95

  * 1 x PIR (motion) sensor[ID:189] = $9.95

  * 1 x 4GB SD card for Raspberry Pi preinstalled with Raspbian Wheezy[ID:1121] = $9.95

  * 1 x Micro USB cable - free - I used an old phone power charger to power the pi board

A total of about $120 after taxes. Expensive for a digital picture frame but relatively cheap after for a hobby.

[caption id="attachment_1541" align="alignright" width="225"][![Raspberry pi](http://jokecamp.files.wordpress.com/2014/06/2014-06-07-16-13-54.jpg?w=225)](https://jokecamp.files.wordpress.com/2014/06/2014-06-07-16-13-54.jpg) Raspberry Pi[/caption]

## My issues and frustrations

**My WIFI dongle is finicky**. It only works when I insert it half-way and it will often drop the connection. I knew when it was working by watching for the flashing blue light. When the light turns a solid blue light the connection no longer works. This was my must frustrating experience. I am unsure if the root of the issue is software, the dongle, or the actual pi. I hope to replace the dongle soon and also try some pi firmware upgrades.

**A bluetooth keyboard and mouse did not work with the pi.** The pi would miss key presses and stutter. The devices were running low on power and the bluetooth was a bad idea. The solution was to use a basic wired USB keyboard and mouse. I ended up picking up a basic USB splitter for about $12. After reading up it looks like there are a lot of advocates for USB hubs with independent power sources but I was hoping to keep not introduce a cumbersome USB hub.

**Remoting into the pi from my mac was tricky at first**. I needed to find the pi's ip address and in my case I needed to do this without having access to the pi. In the end I did this by logging into the remote administration web console for my wireless router. From there I could see a list of connected devices via the "DHCP Reservations" section. Once you have the ip it as a simple as opening the mac terminal and running [code]ssh pi@192.168.1.125[/code] then entering the password.

**The LDC screen was too small for dev work.** After connecting everything and powering on the pi I realized that my 3.5 inch display was going to be too small to read anything. It was a bit anticlimactic and ended my development work for the day. **Roadblocks that ended the days work were common**. Be prepared to have to wait a few days for new parts or to research. I ended up connecting my pi directly to my TV in the living room via my really long HDMI cable. This made it readable but a bit hard and impractical for working on. Luckily, I only needed to use the TV until I could get the WIFI dongle setup. Once connected to the local network I could switch to controlling the pi via my mac laptop using SSH.

**Getting familiar with UNIX**. Some common commands I utilized:

  * sudo reboot
  * sudo halt
  * raspi-config - menu for setting common pi settings. It looks like a BIOS menu
  * startx - start the GUI. I ended up using the terminal more
  * df -h - check the disk space

**Installing the dependent software** to power the python script to download the flickr photos.
Install [pip](https://pypi.python.org/pypi/pip) a python package manager. We will need the flickrarpi and requests packages.

```
wget https://raw.github.com/pypa/pip/master/contrib/get-pip.py
sudo python get-pip.py
# ready to install the packages
sudo pip install flickrapi
sudo pip install requests
```

**Installing fbi - frame buffer imageviewer**. This is the program that actually displays the slideshow.
```sudo apt-get install fbi``` Once fbi is running you can kill it by getting the pid with ```pidof fbi``` and then killing it by ```kill #pid``` where #pid is the process id.

## Fully Operational!

The slideshow is operational now and pulling images directly from my flickr stream. I still have some work to do with a proper display and fixing my wifi issues but the project has been very satisfying. It was fun to make something tangible again. As I have been writing this post I have been quite distracted. Distracted by my new pi slideshow.

[caption id="attachment_1572" align="aligncenter" width="225"][![Final Frame](http://jokecamp.files.wordpress.com/2014/06/2014-06-09-21-53-58.jpg?w=225)](https://jokecamp.files.wordpress.com/2014/06/2014-06-09-21-53-58.jpg) Final Frame[/caption]
