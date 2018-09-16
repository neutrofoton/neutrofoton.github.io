---
layout: post
title: "Configure PHP and Virtual Host macOS High Sierra"
date: 2018-09-16 08:48:43 +0700
comments: true
categories: [macos, apache, php]
---

Apache is pre installed on macOS High Sierra. We just need to run its service with the following command to activate it.

``` bash
$ sudo apachectl start
```
Then we just open <code>http://localhost</code> via browser. Apache will display a default HTML page come with it.

## Activating PHP Module

High Sierra also comes with PHP 7. So we don't need to install it manually. To activate PHP module 

1. Edit <code>/etc/apache2/httpd.conf</code>
2. Uncomment / remove remark of 
   <code>#LoadModule php7_module libexec/apache2/libphp7.so</code>
3. Save it and restart apache using 
    ``` bash
        $ sudo apachectl restart
    ```


After applying the steps above, the php module should be activated and ready to use. In this post we will test it after configuring virtual host. 

## Configuring Virtual Host
The steps of configuring apache virtual host are :

1. Enabling virtual host configuration in apache config by editing 
   <code>/etc/apache2/httpd.conf</code>.

    ``` bash
    $ sudo nano /etc/apache2/httpd.conf
    ```
2. Uncomment the section 
    <code>Include /private/etc/apache2/extra/httpd-vhosts.conf</code>, then save it.

3. Create site directory.
    As an example in this post, let's create a <code>Site</code> folder in home directory called </code>/Users/USERNAME/Sites</code>. Our website sample directory will be put in it, let's create a directory called </code>/Users/USERNAME/Sites/neutro.io</code> and create an <code>/Users/USERNAME/Sites/neutro.io/index.php</code> with simple PHP syntax.

   ``` php
    <?php
    phpinfo();
    ?>
   ```
4. Create virtual host configuration by editing the virtual host config 

   ``` bash
    $ sudo nano /etc/apache2/extra/httpd-vhosts.conf
   ```

    The following code is an example of virtual host with domain name <code>neutro.io</code>

    ``` xml
    <VirtualHost *:80>
        ServerName neutro.io
        ServerAlias www.neutro.io
        DocumentRoot "/Users/neutro/Sites/neutro.io"

    <Directory /Users/neutro/Sites/neutro.io>
            Options Indexes FollowSymLinks
            #Options All Indexes FollowSymLinks
            AllowOverride None
            Require all granted
    </Directory>


        ErrorLog "/private/var/log/apache2/neutro.io-error_log"
        CustomLog "/private/var/log/apache2/neutro.io-access_log" common
        ServerAdmin web@neutro.io
    </VirtualHost>
    ```

    In this example, we create a <code>neutro.io</code> virtual host that refers to <code>/Users/neutro/Sites/neutro.io</code> as physical directory.

5.  Register domain for localhost

    Since we use <code>neutro.io</code> as domain for localhost, we need to add the domain and <code>www</code> alias to resolve to the localhost address by editing 

    ``` bash
        $ sudo nano /etc/hosts
    ```
    and add the following line

    ```
        127.0.0.1   neutro.io   www.neutro.io
    ```

6. Restart apache 

    ``` bash
    $ sudo apachectl restart
    ```

When we open in browser <code>http://neutro.io</code>, we should get a page that display PHP info.


## Losing Default Localhost
After configuring the virtual host, we may lose the previous default localhost that points to <code>/Library/WebServer/Documents/</code> directory. We may get a 403 Forbidden Error when visiting <code>http://localhost</code>. To get around this, we need to add in a vhost for localhost and declare this vhost before any of the others. The following code is our new Virtual host after adding config for localhost.

``` xml
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot /Library/WebServer/Documents/
</VirtualHost>

<VirtualHost *:80>
    ServerName neutro.io
    ServerAlias www.neutro.io
    DocumentRoot "/Users/neutro/Sites/neutro.io"

   <Directory /Users/neutro/Sites/neutro.io>
        Options Indexes FollowSymLinks
        #Options All Indexes FollowSymLinks
        AllowOverride None
        Require all granted
   </Directory>


    ErrorLog "/private/var/log/apache2/neutro.io-error_log"
    CustomLog "/private/var/log/apache2/neutro.io-access_log" common
    ServerAdmin web@neutro.io
</VirtualHost>
```

Restart apache and open <code>http://localhost</code> in browser.

## References
1. https://websitebeaver.com/set-up-localhost-on-macos-high-sierra-apache-mysql-and-php-7-with-sslhttps
2. https://coolestguidesontheplanet.com/set-up-virtual-hosts-in-apache-on-macos-high-sierra-10-13/
