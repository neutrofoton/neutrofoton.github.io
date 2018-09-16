---
layout: post
title: "Install PHP on OS X 10.8 Mountain Lion"
date: 2012-08-09 11:13:16 +0800
comments: true
categories: [macos, php]
---
On the previous post, we have set up apache on OS X Mountain Lion. In this post we will continue to install php module to run with apache on Mountain Lion. PHP actually is shipped in OS X 10.8. The first step is edit <code>httpd.conf</code> in apache directory.

``` bash httpd configuration
sudo vi /etc/apache2/httpd.conf
```

Uncommect (remove #) php module declaration
``` text
LoadModule php5_module libexec/apache2/libphp5.so
```

``` bash restart apache
sudo apachectl restart
```

To see and test PHP create a file with the content, and save in /Users/username/Sites as “test.php”. Then open in browser http://localhost/~username/test.php

``` php test.php
<?php phpinfo(); ?>
```

{% img center /images/post/2012-08-09-php.png %}
