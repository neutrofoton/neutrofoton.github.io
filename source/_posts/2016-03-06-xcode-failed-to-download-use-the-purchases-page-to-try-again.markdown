---
layout: post
title: "Xcode failed to download use the purchases page to try again"
date: 2013-09-24 15:56:27 +0800
comments: true
categories: [macos, xcode]
---
The following day after the release of iOS 7, I started update my iphone 4s to get to know how exactly the ios 7 is. My iphone successfully updated even though it was quite annoying with slow internet connection :( .

The next thing is updating my Xcode, since I got a notification to update my xcode from version 4.6.3 to version 5.0 via app store of version 1.2.2. My OSX version is 10.8.5.
While updatig process, several times I got error message <code>Xcode failed to download use the purchases page to try again</code>, and my download prosess suddenly stopped. Googling on internet and asking forum got many different suggestion and solution. Thankfully one of them success that is by clearing appstore download cache.

The steps to get things to work are quite simple.

<ol>
<li>
Keep the AppStore App open. Open terminal and type
``` bash
cd /private/var/folders/
```

Once there, search for com.apple.appstore
``` bash
find . | grep com.apple.appstore
```
You will find folder structure like this <code>./40/lhn22jn901zdw2bpf82hkggw0000gn/C/com.apple.appstore</code>

</li>

<li>
Once inside the folder, open it in finder
``` bash
open .
```
</li>
<li>
While keeping AppStore open, remove this folder
``` bash
rm -rf *
```
</li>
<li>
Now, go back to AppStore and click on Download again.
</li>
<li>
If download/update disappear, close the appstore then reopen it.
 </li>
</ol>

That steps worked well to me. Good luck and have a nice day.

<h4>References</h4>
<ol type="1">
  <li>http://apple.stackexchange.com/questions/61646/xcode-failed-to-download-use-the-purchases-page-to-try-again/71202#71202</li>
</ol>
