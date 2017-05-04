---
layout: post
title: "GroovyDynamicElementReader is Missing Import Spring Framework Source to Eclipse STS"
date: 2016-03-12 16:08:34 +0800
comments: true
categories: [java, spring, eclipse]
---

{% img left /images/logo/spring.png %}
Sometime for some reasons when dealing with open source technology we have interest to download the full source code of the frameworks we use. Either for customization, extending them or just for having a look to have more knowledge how exactly they work.

<a href="https://spring.io/">Spring</a> is a popular framework in Java world. It's one of my favorite framework in Java. At the time of this blog post, I have just grabbed the source of it from <a href="https://github.com/spring-projects/spring-framework">github repository</a>. The source code is fantastic. Nice coding style, project structure, build tool and of course its documentations. While following the guidance to build and import to <a href="https://spring.io/tools">Spring Tool Suite</a>, I got an error on <code>spring-beans-groovy</code> project in <code>GroovyBeanDefinitionReader.java</code>. Navigating to the errors, STS seems try to find a class <code>GroovyDynamicElementReader</code> but not found.

{% img center /images/post/2016-03-12-pic01.png %}

 Honestly I have no any experience with groovy before. When I try to drill through <code>spring-beans-groovy/src/main/groovy/org/springframework/beans/factory/groovy</code>, finally get a clue <code>GroovyDynamicElementReader.groovy</code>.

Installing <a href="https://github.com/groovy/groovy-eclipse/wiki">Groovy Eclipse</a> seems solve the problem.

{% img center /images/post/2016-03-12-pic02.png %}

In case you have the same problem as what I got, just ensure you have <a href="https://github.com/groovy/groovy-eclipse/wiki">Groovy Eclipse</a> on your eclipse base IDE.

<h3>Reference</h3>
<ol>
<li>https://github.com/groovy/groovy-eclipse/wiki</li>
<li>http://blog.csdn.net/zlx510tsde/article/details/46241675</li>
</ol>
