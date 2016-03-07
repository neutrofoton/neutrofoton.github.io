---
layout: post
title: "Install phpMyAdmin on OS X 10.8 Mountain Lion"
date: 2012-08-10 11:39:53 +0800
comments: true
categories: [osx, php]
---
In the previous post, we have installed Apache, PHP and MySQL on OS X 10.8. To do administration of mySQL database we can use various app. One of the tool that usually use by programmer to do that is <a href="http://www.phpmyadmin.net/home_page/index.php">phpMyAdmin</a>. It is a free opensource web app that’s used to manage MySQL database. Let’s install it now.

The first thing that we need to do is open terminal and type

``` bash symbolic link for mysql
sudo mkdir /var/mysql

sudo ln -s /tmp/mysql.sock /var/mysql/mysql.sock

```
Extract the downloaded <code>phpMyAdmin-3.5.2.1-english.zip</code> into web folder and rename as <code>phpMyAdmin</code>. So now we should have <code>~/Sites/phpMyAdmin</code> directory.

On browser, open : http://localhost/~username/phpmyadmin

{% img center /images/post/2012-08-10-phpmyadmin.png %}

Now we can login and administer MySQL database.
