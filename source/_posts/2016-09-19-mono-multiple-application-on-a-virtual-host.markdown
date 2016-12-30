---
layout: post
title: "Mono Multiple Application on a Virtual Host"
date: 2016-09-19 22:06:13 +0700
comments: true
categories: [linux,csharp,mono,apache]
---
{% img left /images/logo/mono.png %}
On my previous post I wrote how to configure application virtual host. We can have multiple application in a virtual host like multiple application in a web site on IIS. To do that, the steps are the same as I did on my previous post. Let's say we have a scenario, having a site config as the sample on previous article, an application port 99, default document root <code>/var/www/vhosts/defaultsite/root</code>. The detail virtual host config as follow


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

The steps to enable site config is the same as privious post. To make virtual host have multiple applications, we have to modify the site config file as follow. In this sample, we are going to add 2 applications to this virtual host

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

In the new virtual host configuration we have:

1. <code>/project1</code>, with physical application's files in <code>/home/neutro/Workspace/dotnet/project1</code>
2. <code>/project2</code>, with physical application's files in <code>/home/neutro/Workspace/dotnet/project2</code>

Let's assume we have a simple <code>index.aspx</code> file in <code>/home/neutro/Workspace/dotnet/project1</code> and <code>/home/neutro/Workspace/dotnet/project2</code>

We need to give access to both folders and theirs file contents

``` bash
$ find project1 -type d -exec chmod 755 {} \;
$ find project1 -type f -exec chmod 644 {} \;

$ find project2 -type d -exec chmod 755 {} \;
$ find project2 -type f -exec chmod 644 {} \;

```

After modifying the config file and give access to the application directories, we need to register the applications in <code>/etc/mono-server4/debian.webapp</code>

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

The final step is restart apache, and now we should be able to open in browser http://localhost:99/project1 or http://localhost:99/project2.

## References
1. http://stackoverflow.com/questions/19279286/fail-to-start-a-mono-site-with-apache2-404-error-with-mod-mono
