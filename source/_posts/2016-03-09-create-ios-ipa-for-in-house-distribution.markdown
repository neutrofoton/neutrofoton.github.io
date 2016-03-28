---
layout: post
title: "Create iOS ipa for In-House Distribution"
date: 2016-03-09 00:10:44 +0800
comments: true
categories: [osx, ios, xcode]
---

{% img left /images/logo/ios.png %}

Couple of month ago in the era of iphone 6, the company I work for was officially migrate official mobile device from BlackBerry to iphone 6. By the time of migration, development team also got initiative request from users to create an app for android. On the other hand, we also want to anticipate another request for ios platform. To simplify the porting to another platform, we use <a href="https://en.wikipedia.org/wiki/HTML5">HTML5</a>, <a href="http://www.telerik.com/kendo-ui"/>Kendo UI</a> and <a href="https://cordova.apache.org/">Apache Cordova</a> to build the app. The app just consumes data from REST services. The other requirement is the app will not be published to App Store. We have internal mobile device management (MDM) called <a href="https://www.mobileiron.com/"/>MobileIron</a>.

In this post, I will not write about how to build the app with Apache Cordova, I just want to focus on how to create ipa distribution package for in house enterprise distribution. The steps should be the same for app that is created with native. I also will not discuss about how to deploy to MDM.

The first requirement is we have to be a member of <a href="https://developer.apple.com/programs/enterprise/">iOS Developer Enterprise Program</a> which costs $299. The steps how to register is clearly explained in apple developer site.

At the time of write this post, my development environment :
<ol type="1">
<li> OS X El Capitan (10.11.3)</li>
<li>Xcode (7.2)</li>
<li>iphone 6 (iOS 9.2)</li>
</ol>

Before we start create ipa package, ensure our code runs well as expected on iphone simulator.

The steps of creating ipa package for distribution basically similar to deploying to real device for development as explained on <a href="{% post_url 2016-03-08-deploying-ios-app-to-real-device %}">my previous post</a>.
In this post, I will only capture the things that are difference between them.

<h2>Certificate</h2>
While creating certificate preparation in <a href="https://developer.apple.com/account/ios/certificate/create">Apple Developer</a> portal, showed its slightly difference from <a href="{% post_url 2016-03-08-deploying-ios-app-to-real-device %}">my previous post</a>
{% img center /images/post/2016-03-09-pic01.png %}

In this case, I choose <code>In-House and Ad Hoc </code> distribution option instead of <code>iOS App Development</code>, since we plan to create ipa package for In House distribution. If we notice upon the above screenshot, in the <code>Production</code> section has option for <code>In-House and Ad Hoc </code> distribution. Meanwhile on <a href="{% post_url 2016-03-08-deploying-ios-app-to-real-device %}">my previous post</a> has <code>App Store and Ad Hoc</code>. The difference because of the apple id I used on this post is enrolled as an <a href="https://developer.apple.com/programs/enterprise/">iOS Developer Enterprise Program</a>, and on <a href="{% post_url 2016-03-08-deploying-ios-app-to-real-device %}">my previous post</a> uses Individual Apple Developer Program. The detail difference between them and other developer license schema can found <a href="https://developer.apple.com/support/compare-memberships/">here</a>.

The rest of preparing certificate installation for production is the same as <a href="{% post_url 2016-03-08-deploying-ios-app-to-real-device %}">certificate installation for development</a>.

<h2>App IDs</h2>
The step of registering app <code>Bundle ID</code> is the same as <a href="{% post_url 2016-03-08-deploying-ios-app-to-real-device %}">the previous post</a>. If we already registered for development, we should not do it again.

<h2>Provisioning Profiles</h2>
Creating provisioning also similar to the creation Provisioning for Development. Except at the wizard option we need to choose one of in the <code>Distribution</code> group instead of <code>Development</code> group. In this sample I select <code>In House</code>, since the ipa package we created will be deployed via MDM.

{% img center /images/post/2016-03-09-pic02.png %}

The next step is opening Xcode and add apple id via Xcode <code>Proferences > Accounts</code> as I did on <a href="{% post_url 2016-03-08-deploying-ios-app-to-real-device %}">previous post</a>.
Now we should get the similar picture as follow.

{% img center /images/post/2016-03-09-pic03.png %}

Ensure <code>distribution profile</code> already downloaded. If not we can click download button on the list above of picture.

The next thing is opening iOS project with Xcode and ensuring we select <code>Distribution Profile</code> we installed on <code>Code signing</code> section group both on <code>Project</code> and <code>Target</code>.


{% img center /images/post/2016-03-09-pic04.png %}
{% img center /images/post/2016-03-09-pic05.png %}

The next step is select <code>Product</code> menu > <code>Archive</code>, then follow the wizard.

{% img center /images/post/2016-03-09-pic06.png %}
{% img center /images/post/2016-03-09-pic07.png %}
{% img center /images/post/2016-03-09-pic08.png %}
{% img center /images/post/2016-03-09-pic09.png %}
{% img center /images/post/2016-03-09-pic10.png %}

When the wizard finish, by default Xcode will create ipa package for us on desktop.

That's all my share and thanks for reading :)
