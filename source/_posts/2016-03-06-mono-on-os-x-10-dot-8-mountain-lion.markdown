---
layout: post
title: "Mono on OS X 10.8 Mountain Lion"
date: 2012-09-19 13:54:14 +0800
comments: true
categories: osx, c#
---
In this post I will write about setup and configure mono on OS X. I need mono on my Mac since C# is one of my favorite programming language ;)

First of all let’s download <a href="http://www.go-mono.com/mono-downloads/download.html">Mono SDK and MonoDevelop</a>. The order installation is Mono SDK first then MonoDevelop. The installation of Mono SDK and MonoDevelop should be easy by following the installation wizard.

When the installation finish, open terminal :

``` bash mono version
mono -V
```

We should get info about the version of mono installed in our machine.

{% img center /images/post/2012-09-19-pic0.png %}

Try to create an ASP.NET Web Application project by selecting menu :
<code>File > New > Solution</code>. Select <code>C# > ASP.NET > Web Application</code>

{% img center /images/post/2012-09-19-pic1.png %}

Click <code>Run > Run With > Mono Soft Debugger for ASP.NET</code>

{% img center /images/post/2012-09-19-pic2.png %}


<h2>ASP.NET on Apache Web Server</h2>

To run ASP.Net on Apache web server, we need <a href="http://www.mono-project.com/Mod_mono">Mod mono</a> module. It is an Apache 2.0/2.2 module that provides ASP.Net support for Apache web server. The steps installation are :
<ol type="1">
<li>Install Apache web server. The steps for OS X Mountain Lion can be found in my previous post
</li>

<li>Install Xcode, OS X development tool. It’s free and can be downloaded via App Store. We need it to compile mod mono installation source
</li>

<li>Download Mod mono source installation here. At the time of write this article I use <code>mod_mono-2.10.tar.bz2</code>
</li>

<li>Install mod mono module. Open terminal

``` bash mod mono installation
mkdir ~/modmono
cp ~/Downloads/mod_mono-2.10.tar.bz2 ~/modmono
cd ~/modmono
tar xzf mod_mono-2.10.tar.bz2
cd mod_mono-2.10
./configure
make
sudo make install
sudo cp /etc/apache2/mod_mono.conf /etc/apache2/other
```
</li>

<li>Add <code>mod_mono.conf</code> reference in <code>httpd.conf</code>
``` bash edit httpd.conf
sudo vi /etc/apache2/httpd.conf
```
Add the following code at the end of <code>httpd.conf</code>
``` bash httpd.conf
Include /etc/apache2/mod_mono.conf
```
</li>

<li>Create a web directory with the path<code> ~/Projects/Mono/TestMonoApache/TestMonoApache/ </code>

In that directory create an <code>index.aspx</code> file
``` aspx-cs index.aspx
<center>mod_mono is working:<%=System.DateTime.Now.ToString()%></center>
```
</li>
<li>Add an Apache configuration file for mono.
``` bash edit mod_mono.conf
sudo vi /etc/apache2/mod_mono.conf
```
Add the following line at the end of <code>mod_mono.conf</code>
``` xml mod_mono.conf
Alias /testmono "/Users/username/Projects/Mono/TestMonoApache/TestMonoApache/"
<Directory "/Users/username/Projects/Mono/TestMonoApache/TestMonoApache/">
    Options Indexes FollowSymLinks MultiViews
    AllowOverride None

    Order allow,deny
    Allow from all
</Directory>
AddMonoApplications default "/testmonoapache:/Users/username/Projects/Mono/TestMonoApache/TestMonoApache/"
<Location /testmonoapache>
 SetHandler mono
</Location>
```
</li>

<li>Restart apache server with command :
``` bash restart apache
sudo /usr/sbin/apachectl restart
```
</li>

<li> Open browser and hit <code>http://localhost/testmonoapache/index.aspx</code>
{% img center /images/post/2012-09-19-pic3.png %}
</li>

</ol>

Cool, now our apache web server can receive request for asp.net page. Thanks for reading and see you in the next post :)


<h4>References</h4>
<ol type="1">
  <li>http://www.mono-project.com/Mod_mono</li>
  <li>http://blog.coultard.com/2012/04/developing-using-c-and-mono-on-mac.html</li>
  <li>http://www.ienablemuch.com/2010/10/aspnet-on-mac-os-x-snow-leopard-at-one.html</li>
</ol>
