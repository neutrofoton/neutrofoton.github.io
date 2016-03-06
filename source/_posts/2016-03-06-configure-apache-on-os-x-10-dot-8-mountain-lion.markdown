---
layout: post
title: "Configure Apache on OS X 10.8 Mountain Lion"
date: 2012-08-08 10:47:05 +0800
comments: true
categories: osx, apache
---
After installing Mountain Lion on my Macbook, I decided to prepare and set up software development tools in it. One of it is activate web sharing feature which can be found at System Preferences > Sharing on previous version. I was very suprise that apple seems remove it from the list.

{% img center /images/post/2012-08-07-pic01.png %}

By searching some references on internet, here are the things that I did. Apache actually is pre-installed on Mountain Lion and we just need to enable it via the command line.

``` bash start apache server
sudo apachectl start

```

We will be asked to enter login password for starting it. Enter a password then open in browser localhost website. You should get the following display

{% img center /images/post/2012-08-07-pic02.png %}

``` bash stop apache server
sudo apachectl stop
```

``` bash get apache version
httpd -v
```
Historically, OSX has had 2 web roots. One at a system level and the other one at a user level. The user level one allows multiple acounts to have their own web root whilst the system one is global for all users. The location of system web document root is at

``` text
/Library/WebServer/Documents/
```
On the other hand, the user level web roots in the previous version of OSX (Lion) can be found under ~/Sites. But we can not find that directory anymore in Mountain Lion. So we need to create it manually.

{% img center /images/post/2012-08-07-pic03.png %}

In terminal type:
``` bash edit configuration
cd /etc/apache2/users
vi username.conf
```

Edit username.conf in vi editor as is the following code:

``` text
<Directory "/Users/username/Sites/">
Options Indexes MultiViews
AllowOverride None
Order allow,deny
Allow from all
</Directory>
```

Then create html page inside ~/Sites to test our user local website and save it as index.html
``` html sample page
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <meta http-equiv="Content-Style-Type" content="text/css">
  <title></title>
  <meta name="Generator" content="Cocoa HTML Writer">
  <meta name="CocoaVersion" content="1187">
  <style type="text/css">
  </style>
</head>
<body>
<h1><b>It works!</h1>
<h1><b>I am on local user website</b></h1>
</body>
</html>

```
{% img center /images/post/2012-08-07-pic04.png %}

Congratulation your apache is running well now :)
