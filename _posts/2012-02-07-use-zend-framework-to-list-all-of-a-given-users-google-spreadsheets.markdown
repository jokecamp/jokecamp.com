---
author: jokecamp
comments: true
date: 2012-02-07 23:14:39+00:00
layout: post
slug: use-zend-framework-to-list-all-of-a-given-users-google-spreadsheets
title: Use Zend Framework to List all of a given user's Google Spreadsheets
desc: 'Example code of using the Zend framework to list all Google spreadsheets of a given user'
wordpress_id: 104
categories:
- Code
tags:
- googleapi
- php
- zend
---

Get a list of all of a given user's google spreadsheets and get the keys. You will need to know the username/password and plug them in below.

This took me more time than I would want to admit. Being new to all the involved parties of PHP, [Zend Framework](http://framework.zend.com/manual/en/zend.gdata.spreadsheets.html) and [Google Spreadsheets API](http://code.google.com/apis/spreadsheets/) did not help. The service returns a lot of data from the getSpreadsheetFeed() call and it took some time to understand.


```php
<?php

$path = $_SERVER['NFSN_SITE_ROOT'] . "protected/Zend/library/";
set_include_path($path);

require_once 'Zend/Loader.php';
Zend_Loader::loadClass('Zend_Gdata_AuthSub');
Zend_Loader::loadClass('Zend_Gdata_ClientLogin');
Zend_Loader::loadClass('Zend_Gdata_Spreadsheets');
Zend_Loader::loadClass('Zend_Gdata_Docs');

$username = 'xxx'; // Your google account username
$password = 'xxx'; // Your google account password

$service = Zend_Gdata_Spreadsheets::AUTH_SERVICE_NAME;
$client = Zend_Gdata_ClientLogin::getHttpClient($username, $password, $service);
$spreadSheetService = new Zend_Gdata_Spreadsheets($client);
$feed = $spreadSheetService->getSpreadsheetFeed();

foreach ($feed->entry as $entry)
{
   echo "name = " . $entry->content->text . "<br>";
   echo "url = " . $entry->id->text . "<br><br>";
}

?>
```
