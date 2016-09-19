---
layout: post
title: "ASP.Net MVC 4 in Xamarin Studio"
date: 2013-11-23 16:15:03 +0800
comments: true
categories: [csharp, xamarin, mono]
---
<a href="http://xamarin.com/studio">Xamarin Studio</a> is a great Mono IDE (Integrated Development Environment) in OS X area. At the time of writing this article, the version of Xamarin studio and mono framework I use are :
<code>
Xamarin Studio
Version 4.2.1 (build 1)
Runtime:
Mono 3.2.5 ((no/964e8f0)
GTK+ 2.24.20 theme: Raleigh
GTK# (2.12.0.0)
Package version: 302050000
</code>

Xamarin Studio includes with ASP.NET MVC 2 and 3 project template. While trying to create ASP.NET MVC 3 project, I got couples of errors such as authorization access etc. The good news is even though there is no project template for ASP.NET MVC 4, we still can convert our ASP.NET MVC 3 to run as ASP.NET MVC 4. The steps are :

<ol type="1">
<li>
Create an ASP.NET MVC 3 project either with aspx or razor view engine
</li>
<li>
Run the project via Xamarin studio menu (Run > Run With > Mono Soft Debugger for ASP.NET. While running the project, we may got an error message :

``` text
System.UnauthorizedAccessException
Access to the path "/Library/Frameworks/Mono.framework/Versions/3.2.5/etc/mono/registry" is denied.

Description: HTTP 500.Error processing request.
Details: Non-web exception. Exception origin (name of application or object): mscorlib.
Exception stack trace:

......

```
To solve the error, run the following command in terminal :
``` bash
sudo mkdir /Library/Frameworks/Mono.framework/Versions/3.2.5/etc/mono/registry
sudo chmod g+rwx /Library/Frameworks/Mono.framework/Versions/3.2.5/etc/mono/registry
```
</li>

<li>
Re-run again the ASP.NET MVC project. We may get another error message.
``` text
System.IO.FileNotFoundException
Could not load file or assembly 'System.Web.WebPages, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.

Description: HTTP 500.Error processing request.
Details: Non-web exception. Exception origin (name of application or object): mscorlib.
Exception stack trace:

...
```

To solve the error message, just use NuGet package manager, search for “ASP.NET MVC 4″ and add reference to it and its dependencies.

</li>
<li>
Re-run the ASP.NET MVC again. Now we may get another error message as following :

``` text
System.InvalidOperationException
Conflicting versions of ASP.NET Web Pages detected: specified version is "1.0.0.0", but the version in bin is "2.0.0.0". To continue, remove files from the application's bin directory or remove the version specification in web.config.

Description: HTTP 500.Error processing request.
Details: Non-web exception. Exception origin (name of application or object): System.Web.WebPages.Deployment.
Exception stack trace:

...

```

As the error message, we need to make our ASP.NET Web Pages version to 2.0.0.0 in web.config. So just open web.config and change webpages:Version to 2.0.0.0. as follow :

``` xml web.config
<add key="webpages:Version" value="2.0.0.0">
</add>
```
</li>

<li>
Change assembly version declared in root <code>web.config</code> and <code>/View/web.config</code> of the following

<table style="width:80%">
  <tr>
    <td colspan="3"> <hr/> </td>
  </tr>
  <tr>
    <td>assembly</td>
    <td>from</td>
    <td>to</td>
  </tr>
  <tr>
    <td colspan="3"> <hr/> </td>
  </tr>
  <tr>
    <td><code>System.Web.WebPages.Razor</code></td>
    <td>1.0.0.0</td>
    <td>2.0.0.0</td>
  </tr>
  <tr>
    <td><code>System.Web.Mvc</code></td>
    <td>3.0.0.0</td>
    <td>4.0.0.0</td>
  </tr>
</table>

</li>
</ol>

Now re-run the project again via Xamarin Studio and our project should run well as expected.

<h4>References</h4>
<ol type="1">
<li> http://curtis.schlak.com/2013/09/29/setup-asp-net-mvc-4-on-monodevelop.html
</li>
<ol>
