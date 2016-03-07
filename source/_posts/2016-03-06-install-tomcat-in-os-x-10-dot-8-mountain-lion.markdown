---
layout: post
title: "Install Tomcat in OS X 10.8 Mountain Lion"
date: 2012-09-14 13:36:00 +0800
comments: true
categories: [osx, apache, java]
---
In this blog post will show us about step installing Apache Tomcat on OS X Mountain Lion. I assume we already have Java installed on our machine. The first step is downloading Tomcat binary distribution in <a href="http://tomcat.apache.org/">Tomcat website</a>. In this case I use version 7.0.30.

Extract the downloaded binary distribution onto <code>~/Download/apache-tomcat-7.0.30</code>.
Create directory in <code>/usr/local</code> if we don’t have it yet.

``` bash tomcat installation
sudo mkdir /usr/local
```

Move <code>~/Download/apache-tomcat-7.0.30 </code> to <code>/usr/local</code>

``` bash tomcat installation
sudo mv ~/Downloads/apache-tomcat-7.0.30 /usr/local
```

To make it easy to replace this release with future releases, we are going to create a symbolic link that we are going to use when referring to Tomcat. Then we also will change owner and make script executable

``` bash tomcat installation
sudo ln -s /usr/local/apache-tomcat-7.0.30/ /Library/Tomcat
sudo chown -R neutrocode /Library/Tomcat
sudo chmod +x /Library/Tomcat/bin/*.sh
```

The next step is creating user for tomcat admin by editing <code>tomcat-user.xml</code>
``` bash tomcat installation
vi /Library/Tomcat/conf/tomcat-users.xml
```

``` xml tomcat-users.xml
<role rolename="admin-gui"/>
<role rolename="admin-script"/>
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>

<user username="admin" password="password" roles="admin-gui,admin-script" />
<user username="manager" password="password" roles="manager-gui,manager-script,manager-jmx,manager-status" />
```

Add the following text in current user profile .bash_profile
``` bash .bash_profile
export CATALINA_HOME=/Library/Tomcat
export JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home

export PATH=$JAVA_HOME:$PATH
```

Start the server with the following command :
``` bash start tomcat
$CATALINA_HOME/bin/catalina.sh start
```
or

``` bash start tomcat
$CATALINA_HOME/bin/startup.sh
```

{% img center /images/post/2012-09-14-tomcat.png %}

Stop the server with the following command :
``` bash start tomcat
$CATALINA_HOME/bin/catalina.sh stop
```
or

``` bash start tomcat
$CATALINA_HOME/bin/shutdown.sh
```
