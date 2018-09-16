---
layout: post
title: "Reset Git Credential on OS X Keychain"
date: 2016-11-09 12:40:34 +0700
comments: true
categories: [macos,git]
---

{% img left /images/logo/git.png %}
This post basically just a note for my self. In case I got the same condition in the next, it can help me to remember what I did in the past.

I got a condition which I have just changed my git password. Surely I cannot pull or push since my local credential was not valid anymore. When I run <code>git config --list</code>, I believe my git password stored on OS X Keychain. The simple way I do was running a command via terminal :

``` bash
$ git config credential.helper osxkeychain
```

I was then prompted to input my username and password. By inputing my username and new password everything back to normal.

If you want it to apply globally just append parameter <code> --global </code> to the command.
``` bash
$ git config --global credential.helper osxkeychain
```

## References
1. http://stackoverflow.com/questions/20195304/how-to-update-password-for-git
