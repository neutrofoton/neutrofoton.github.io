---
layout: post
title: "Mono mod_mono Apache2 on Ubuntu"
date: 2016-09-18 08:34:23 +0700
comments: true
categories:
---
Previously I used to write a post about install and configure Mono on OS X Mountain Lion. On this post I want to summary what I did on Ubuntu. The details environment I use :

1. Ubuntu 14.04 LTS
2. Mono JIT compiler version 4.4.2
3. Apache 2.4.7


## Install Mono, Apache 2, Mod Mono
To install Mono, the first step in add package repository to our system.

``` bash
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF

$ echo "deb http://download.mono-project.com/repo/debian wheezy main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list

$ sudo apt-get update

```

To enable mod_mono installation on Ubuntu 13.10 and later, and Debian 8.0 and later (and their derivatives), we need to add a second repository to our system, in addition to the generic Debian/Ubuntu repository above (if you don’t use sudo, be sure to switch to root):

``` bash

$ echo "deb http://download.mono-project.com/repo/debian wheezy-apache24-compat main" | sudo tee -a /etc/apt/sources.list.d/mono-xamarin.list

```

Then run package update to update and a package upgrade to upgrade existing packages to the latest available.

``` bash
$ sudo apt-get update
$ sudo apt-get upgrade

```

To install mono run the following command

``` bash install mono
$ sudo apt-get install mono-complete
```

If we have not Apache 2 on our system, we need to install it first. Or we can check exist version <code>apache2 -v</code>

``` bash install apache2
$ sudo apt-get install apache2
```

To be able to host ASP.NET application on apache, we need to install mod_mono. mod_mono is a module for the Apache HTTP Server that allows for hosting of ASP.NET pages and other assemblies on multiple platforms by use of Mono.

To install Mod Mono and its dependencies run the following command

``` bash install mod_mono
$ sudo apt-get update && sudo apt-get install libapache2-mod-mono
$ apt-get install mono-apache-server4
```

Before going through the next step, the things than we can verify are :

1. Ensuring Apache 2 running well at least by checking its version or start stop its service.

    ``` bash apache version
    $ apache2 -v
    ```

    ``` bash start apache
    $ service apache2 start
    ```

    ``` bash stop apache
    $ service apache2 stop
    ```

2. Checking mono version

   ``` bash mono version
   $ mono --version
   ```
3. Ensuring we have <code>/usr/bin/mod-mono-server4</code>


## Configuring Application Virtual Host

To configure virtual host application, the steps are

1. Create a directory where the application physical files reside.
   In this sample I put my  application files in <code>/var/www/hellodotnet</code>

   We needed to grant permissions to the login user so that the user could copy or create files to the <code>hellodotnet</code> directory.  For our purposes we made the current user the owner of the directory

    ``` bash application directory
    $ chown -R -v ubuntu /var/www/
    ```

2. Lets create a simple <code>/var/www/hellodotnet/index.aspx</code> file.

    ``` html index.aspx
    <center>mod_mono is working:<%=System.DateTime.Now.ToString()%></center>

    ```

3. Create sites configuration <code>/etc/apache2/sites-available/hellodotnet.conf</code>

    ``` xml site configuration
       <VirtualHost *:81>
          ServerName hellodotnet
          ServerAdmin hello@test-apache-config.com
          DocumentRoot /var/www/hellodotnet
          MonoServerPath hellodotnet "/usr/bin/mod-mono-server4"
          MonoDebug hellodotnet true
          MonoSetEnv hellodotnet MONO_IOMAP=all
          MonoApplications hellodotnet "/:/var/www/hellodotnet"

          ErrorLog ${APACHE_LOG_DIR}/error.log
          CustomLog ${APACHE_LOG_DIR}/access.log combined

          AddHandler  mono    .aspx .ascx .asax .ashx .config .cs .asmx .axd
          <Location "/">
            Allow from all
            Order allow,deny
            MonoSetServerAlias hellodotnet
            SetHandler mono
            SetOutputFilter DEFLATE
            SetEnvIfNoCase Request_URI "\.(?:gif|jpe?g|png)$" no-gzip dont-vary
          </Location>
          <IfModule mod_deflate.c>
            AddOutputFilterByType DEFLATE text/html text/plain text/xml text/javascript
          </IfModule>
        </VirtualHost>
    ```

   In this site I use port 81 instead of default 80.

4. Since we use port 81, we need to register port 81 in <code>/etc/apache2/ports.conf</code> by simply add the following code

    ``` bash
    Listen 81
    ```
5. Enabling the site by running the following command from terminal

    ``` bash
    $ sudo a2ensite hellodotnet.conf
    ```
6. Restart apache
7. Hit from browser : http://localhost:81
   We should have the page displayed current date time.

## Tips
1. If we get the page not displayed, we can see the message on the page displayed, HTML error code, or any clue on apache logs located in <code>/var/log/apache2</code>

2. If we get message access denied cannot access bin, simple change permission of the application site.

    ``` bash
    $ find hellodotnet -type d -exec chmod 755 {} \;
    $ find hellodotnet -type f -exec chmod 644 {} \;
    ```

3. If getting message could not find <code>/etc/mono/registry</code>, simply create the directory.

    ``` bash
    $ sudo mkdir /etc/mono/registry
    ```

4. If getting System.UnauthorizedAccessException Access to the path <code>/etc/mono/registry</code>, give access to it.

     ``` bash
     $ chmod uog+rw /etc/mono/registry
     ```

## References
1. http://www.mono-project.com/docs/getting-started/install/linux/
2. http://stackoverflow.com/questions/34290004/how-to-install-configure-mod-mono-on-ubuntu-14-04-3-lts
3. http://www.bloomspire.com/blog/2015/3/9/how-to-host-aspnet-applications-on-linux-p3
4. http://superuser.com/questions/882594/permission-denied-because-search-permissions-are-missing-on-a-component-of-the-p
5. https://www.codementor.io/tips/7134438372/access-to-the-path-etc-mono-registry-is-denied
6. https://retkomma.wordpress.com/2011/10/01/registry-settings-in-mono-on-linux/
