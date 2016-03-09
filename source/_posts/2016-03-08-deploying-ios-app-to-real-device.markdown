---
layout: post
title: "Deploying iOS app to Real Device"
date: 2016-03-08 13:35:31 +0800
comments: true
categories: [osx, ios]
---

While developing ios app, sometime we don't just testing run the app on ios simulator we also need to test the app on real device though. The are couple of things that we need to prepare to make our app be able to run on real devices.


<h2>Certificate</h2>
Development certificate basically tells us that we are apple developers. To be it, our apple id has to be enrolled as an apple developer. Information about apple developer enrollment can be found at <a href="https://developer.apple.com/programs/enroll/">Apple Developer site</a>.

Once our apple id enrolled as an apple developer, there are couple of steps for preparing development certificate. App need to be signed to be able to deployed to real devices. To do that:

<ol type="1">

<li>Go to <code>Launchpad</code> or <code>Spotlight Search</code>, type <code>Keychain Access</code> then open the app.
</li>

<li>At top menu, click <code>Keychain Access</code> > <code>Certificate Assistance</code> > <code>Request a Certificate from a Certificate Authority</code>

{% img center /images/post/2016-03-08-pic01.png %}

Fill the <code>Email</code> and <code>Common Name</code>. The email should be <code>apple id email</code> then select <code>save to disk</code> and checked <code>Let me specify key pair information</code>. When we click continue, we should get save dialog

{% img center /images/post/2016-03-08-pic02.png %}

Select Key Size <code>2048 bits</code> and <code>RSA</code> algorithm, click continue then done.
{% img center /images/post/2016-03-08-pic03.png %}

The certificate request that we create just saved to disk (desktop in my case).

</li>

<li>
Open <a href="https://developer.apple.com/membercenter/index.action">apple developer portal</a>, login using apple id that already enrolled as apple developer.
{% img center /images/post/2016-03-08-pic04.png %}

click <code>Certificates, Identifiers and Profiles</code>

</li>
<li>
Select <code>Sertificates</code> group > <code>All</code>
{% img center /images/post/2016-03-08-pic05.png %}

On the right pane, select <code>add (+)</code> button, select type of certificate that we want to create. At this post we will create for development only. Not for production such as for deploy our app in app store nor internal store (in-house) like mobile device management.
{% img center /images/post/2016-03-08-pic06.png %}

If everything already done on this page, scroll down > click <code>continue</code>

We may notice a information message that we have to ensure on our OS X development machine keychain installed <code>
Worldwide Developer Relations Certificate Authority</code>

{% img center /images/post/2016-03-08-pic07.png %}

</li>
<li>
Follow the wizard, till we're asked to upload our certificate request that we already created on previous step.

{% img center /images/post/2016-03-08-pic08.png %}

</li>
<li>
After completed the wizard, we should be able to download the generated certificate and ready to install on development machine.
{% img center /images/post/2016-03-08-pic09.png %}

</li>
<li>
Download the <code>development certificate</code> > double click. The certificate should be installed on our <code>Keychain Access</code>
{% img center /images/post/2016-03-08-pic10.png %}

<code>
NOTE :
Please ignore the distribution certificate on my screenshot. It just another certificate for distribution which is out of scope of this post.
</code>
</li>
</ol>


<h2>Devices</h2>
This part informs to the xcode that the device is belongs to me. So let me run my app on it. To register our devices, select <code>Devices</code> on the apple developer portal as we did for certificate section above.

{% img center /images/post/2016-03-08-pic11.png %}

Click <code>add (+)</code>
{% img center /images/post/2016-03-08-pic12.png %}

Enter <code>Name</code> and <code>UUID</code>. To get <code>UUID</code>, plug idevice to computer, itune will display it. If itune display <code>Serial Number</code> or <code>ECID</code> or <code>Product Type</code>,  click the label till <code>UUID</code> displayed. Just copy it and paste in the <code>UUID</code> text box above.

{% img center /images/post/2016-03-08-pic13.png %}

Follow the wizard till finish. We should be able to get our idevices listed on the device list.
{% img center /images/post/2016-03-08-pic14.png %}


<h2>App IDs</h2>
On Apple developer portal mentioned that an <code>App ID</code> is a string that defines both a keychain identity and a set of apps you are developing. Its primary use is as part of a provisioning profile, it specifies which apps are authorized by the profile to be signed and launched.

To register <code>App ID</code>, click <code>Add (+)</code>. Fill <code>Name</code> and <code>Bundle ID</code>.

{% img center /images/post/2016-03-08-pic15.png %}

In this sample, my application <code>Name</code> is <code>HelloWorld</code> and <code>Bundle ID</code> is <code>com.neutrofoton.helloworld</code>

Click continue till the wizard dialog finished.
We can get list all of <code>App IDs</code> that we registered by clicking  the <code>App IDs</code> on the left menu panel

{% img center /images/post/2016-03-08-pic16.png %}

<h2>Provisioning Profiles</h2>
On Apple Developer portal Provisioning Profiles defined that it allows us to install apps onto our iOS devices. A provisioning profile includes signing certificates, device identifiers, and an App ID. Development provisioning profiles are used to build and install versions of our app during the development cycle, while distribution provisioning profiles are used to submit our apps to the App Store and distribute them to beta testers.

To start create provisioning, click <code>add (+)</code> select <code>iOS App Development</code>. In this case I will create provisioning for test. For distribution we select on distribution group option as our need.

{% img center /images/post/2016-03-08-pic17.png %}

Follow the wizard by selecting <code>App ID</code>, <code>Devices</code>, and <code>Certificates</code> that will be applied to the <code>Provisioning Profile</code>

{% img center /images/post/2016-03-08-pic18.png %}
{% img center /images/post/2016-03-08-pic19.png %}
{% img center /images/post/2016-03-08-pic20.png %}
{% img center /images/post/2016-03-08-pic21.png %}
{% img center /images/post/2016-03-08-pic22.png %}

Now open <code>Xcode > Preferences </code> select <code>Accounts</code> tab. Add apple id that we use for login to apple developer portal.

Select the apple id on the left pane > click <code>View Details</code> button.
We should get the similar picture as below.

{% img center /images/post/2016-03-08-pic23.png %}

If the profiles are not downloaded yet, we can click <code>Download</code> button that make Xcode do for us.


To test provisioning we created, let's create simple application. At the time of write this post, my development environment :
<ol type="1">
<li> OS X El Capitan (10.11.3)</li>
<li>Xcode (7.2)</li>
<li>iphone 4s iOS 9.1</li>
</ol>

{% img center /images/post/2016-03-08-pic24.png %}
{% img center /images/post/2016-03-08-pic25.png %}

Ensure we have the same <code>Bundle Identifier</code> as we registered on <code>Provisioning Profile</code>. At this sample app, I just create a simple lable <code>Hello World Application</code> on storyboard.

Now run the application by selecting <code>Device</code> instead of <code>Simulator</code>. We may get the following message while running on real device, just click <code>Allow</code>

{% img center /images/post/2016-03-08-pic26.png %}

{% img center /images/post/2016-03-08-pic27.png %}

The above picture shows that the Hello World app can be run on real device.
