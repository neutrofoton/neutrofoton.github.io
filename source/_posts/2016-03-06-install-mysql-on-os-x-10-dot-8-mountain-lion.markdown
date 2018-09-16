---
layout: post
title: "Install MySQL on OS X 10.8 Mountain Lion"
date: 2012-08-09 11:23:09 +0800
comments: true
categories: [macos, mysql]
---
To install mysql the first thing that we need to do is download mysql installer on the <a href="http://dev.mysql.com/downloads/mysql/">mysql website</a>. In that web site we can see several versions of mysql. We will use Mac OS X ver. 10.6 (x86, 64-bit), DMG Archive. When we open the <code>mysql-5.5.27-osx10.6-x86_64.dmg</code> file, we will find 3 package installers.

{% img center /images/post/2012-08-09-msqlpkg.png %}

Install all the three packages with the following order :

1. mysql-5.5.27-osx10.6-x86_64.pkg
2. MySQLStartupItem.pkg
3. MySQL.prefPane

To start/stop MySQL we can use GUI tool from <code>System Preferences > MySQL</code>

{% img center /images/post/2012-08-09-mysql-service.png %}

We can also start/stop MySQL via terminal

``` bash start mysql
sudo /usr/local/mysql/support-files/mysql.server start
```

``` bash stop mysql
sudo /usr/local/mysql/support-files/mysql.server stop
```

``` bash get mysql version
sudo /usr/local/mysql/bin/mysql -v
```

In order to use MySQL command without have to specify MySQL installation full path, we need to add MySQL installation directory to shell path.

``` bash bash profile
cd ~
vi .bash_profile
```

``` bash path
export PATH="/usr/local/mysql/bin:$PATH"
```

Then reload the new PATH

``` bash
source ~/.bash_profile
```

``` bash test get mysql version
mysql -v
```

``` bash set mysql password
cd ~
/usr/local/mysql/bin/mysqladmin -u root password 'yourpassword'
```
