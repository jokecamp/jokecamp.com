---
author: jokecamp
comments: true
date: 2012-07-17 18:30:45+00:00
layout: post
slug: a-clean-javascript-ui-notification-system-with-jquery
title: A Clean Javascript UI Notification System with jQuery
desc: "A very primitive and simple javascript alert/notification project"
wordpress_id: 324
categories:
- Code
tags:
- javascript
- UI
---

[![Javascript UI notifications](http://jokecamp.files.wordpress.com/2012/07/notifications.png)](http://jokecamp.files.wordpress.com/2012/07/notifications.png)

I've created a pretty simple javascript notification system for letting the users know what is going on and what is happening. UIs should be always keep the user informed to what going on. This notification system is particulaly useful for displaying the outcome of AJAX updates. The code uses the jQuery library.

I use three different types of notifications and their respective colors:

  1. Success (Green)
  2. Error (Red)
  3. Information (Blue)

I have the notifications set to pop into the top right corner of the page. Show for a period of time then disapear. The notifications nicely stack on top of each other.

The methods to add the notes:

```javascript
AddInformNote("Caution");
AddSuccessNote("The data was successfully saved.");
AddFailedNote("Error: The record could not be updated.");
```

[View Online Example and Code](http://www.jokecamp.com/lab/javascriptNotifications.html). All the js and CSS source code can be viewed inline in the HTML file.
