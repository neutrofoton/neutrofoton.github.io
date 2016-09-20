---
layout: post
title: "Mono Multiple Application on a Virtual Host"
date: 2016-09-19 22:06:13 +0700
comments: true
categories: [linux,csharp,mono]
---
{% img left /images/logo/mono.png %}
On my previous post I wrote how to configure application virtual host. We can have multiple application in a virtual host like multiple application in a web site on IIS. To do that the steps are the same as I did on my previous post. Let's say I have the following virtual host config

1. Application port 99
2. Default document root <code>/var/www/vhosts/defaultsite/root</code>

``` xml
<VirtualHost *:99>
    ServerAdmin admin@test.com
    ServerName  neutro.local
    ServerAlias neutrofoton.com *.neutrofoton.com

    MonoServerPath neutro.local "/usr/bin/mod-mono-server4"
    MonoDebug neutro.local true
    MonoSetEnv neutro.local MONO_IOMAP=all
    MonoApplications neutro.local "/:/var/www/vhosts/defaultsite/root"

    MonoAutoApplication disabled
    AddHandler  mono    .aspx .ascx .asax .ashx .config .cs .asmx .axd

    DocumentRoot    /var/www/vhosts/defaultsite/root
    DirectoryIndex  Default.aspx index.aspx index.html

    <Location "/">
    	Allow from all
    	Order allow,deny
   	MonoSetServerAlias neutro.local
    	SetHandler mono
    	SetOutputFilter DEFLATE
    	SetEnvIfNoCase Request_URI "\.(?:gif|jpe?g|png)$" no-gzip dont-vary
    </Location>

    <IfModule mod_deflate.c>
      AddOutputFilterByType DEFLATE text/html text/plain text/xml text/javascript
    </IfModule>

    LogLevel    debug

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

We are going to add 2 more applications to this virtual host

1. <code>/project1</code>, with physical application's files in <code>/home/neutro/Workspace/dotnet/project1</code>
2. <code>/project2</code>, with physical application's files in <code>/home/neutro/Workspace/dotnet/project2</code>

To do that we have to modify the site config file as follow
``` xml
<VirtualHost *:99>
    ServerAdmin admin@test.com
    ServerName  neutro.local
    ServerAlias neutrofoton.com *.neutrofoton.com

    MonoServerPath neutro.local "/usr/bin/mod-mono-server4"
    MonoDebug neutro.local true
    MonoSetEnv neutro.local MONO_IOMAP=all
    MonoApplications neutro.local "/:/var/www/vhosts/defaultsite/root"

    MonoAutoApplication disabled
    AddHandler  mono    .aspx .ascx .asax .ashx .config .cs .asmx .axd

    DocumentRoot    /var/www/vhosts/defaultsite/root
    DirectoryIndex  Default.aspx index.aspx index.html

    <Location "/">
    	Allow from all
    	Order allow,deny
   	MonoSetServerAlias neutro.local
    	SetHandler mono
    	SetOutputFilter DEFLATE
    	SetEnvIfNoCase Request_URI "\.(?:gif|jpe?g|png)$" no-gzip dont-vary
    </Location>

    Alias   /project1  "/home/neutro/Workspace/dotnet/project1"
    MonoApplications project1 "/project1:/home/neutro/Workspace/dotnet/project1"
    MonoServerPath project1 "/usr/bin/mod-mono-server4"
    <Location "/project1">
      Allow from all
      Order allow,deny
      MonoSetServerAlias project1
      SetHandler mono
      SetOutputFilter DEFLATE
      SetEnvIfNoCase Request_URI "\.(?:gif|jpe?g|png)$" no-gzip dont-vary
      Require all granted
    </Location>

    Alias   /project2  "/home/neutro/Workspace/dotnet/project2"
    MonoApplications project2 "/project2:/home/neutro/Workspace/dotnet/project2"
    MonoServerPath project2 "/usr/bin/mod-mono-server4"
    <Location "/project2">
      Allow from all
      Order allow,deny
      MonoSetServerAlias project2
      SetHandler mono
      SetOutputFilter DEFLATE
      SetEnvIfNoCase Request_URI "\.(?:gif|jpe?g|png)$" no-gzip dont-vary
      Require all granted
    </Location>

    <IfModule mod_deflate.c>
      AddOutputFilterByType DEFLATE text/html text/plain text/xml text/javascript
    </IfModule>

    LogLevel    debug

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

After modifying the config file, we need to register the application in <code>/etc/mono-server4/debian.webapp</code>

``` xml
<apps>
   <web-application>
        <name>project1</name>
        <vpath>/project1</vpath>
        <path>/home/neutro/Workspace/dotnet/project1</path>
        <vhost>neutro.local</vhost>
    </web-application>
    <web-application>
        <name>project2</name>
        <vpath>/project2</vpath>
        <path>/home/neutro/Workspace/dotnet/project2</path>
        <vhost>neutro.local</vhost>
    </web-application>
</apps>
```

## References
1. http://stackoverflow.com/questions/19279286/fail-to-start-a-mono-site-with-apache2-404-error-with-mod-mono
