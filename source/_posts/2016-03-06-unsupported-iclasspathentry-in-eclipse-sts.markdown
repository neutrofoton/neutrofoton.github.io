---
layout: post
title: "Unsupported IClasspathEntry in Eclipse-STS"
date: 2013-08-30 15:48:31 +0800
comments: true
categories: [java, eclipse]
---
I often got annoying pup up error message. An internal error occurred during updating Maven project. The message was <code>Unsupported IClasspathEntry kind=… </code>

Googling for that problem finally got a trick how to resolve it.
<ol>
<li>Right-click on your project, select <code>Maven > Disable Maven Nature</code></li>
<li>
Open your terminal, go to your project folder and do
``` bash clean eclipse cache
mvn eclipse:clean
```
</li>
<li>
Right click on your Project and select <code>Configure > Convert into Maven Project</code>
</li>

</ol>

After converting into maven project, I can update my maven project and everything running well :)
