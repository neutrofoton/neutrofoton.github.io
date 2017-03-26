---
layout: post
title: "Working with Multiple Github Account on a Computer"
date: 2017-03-26 23:47:22 +0700
comments: true
categories: [git]
---

Source control is one of a basic need for software development, especially when we work on a team. Git is one of popular distributed source control. Working with multiple github account on the same computer need a few tricky way. Let's assume we have cloned the repository from github to local computer. The following ways are the simple ways I got on internet.

## Change Remote URL to HTTPS

This way is by changing the remote URL to HTTPS with the following format.

``` bash
$ git remote set-url origin https://USERNAME@github.com/USERNAME/PROJECTNAME.git

```

Then do normal git operation such as <code>commit</code>, <code>push</code> etc.
To ensure that the commits appear as performed by USERNAME, we can configure the username and email on our working directory.

``` bash
$ git config user.name USERNAME
$ git config user.email USERNAME@example.com

```

## Multiple SSH Key
The other way is by using multiple SSH key. [Here](https://code.tutsplus.com/tutorials/quick-tip-how-to-work-with-github-and-multiple-accounts--net-22574) is the complete tutorial by [Jeffrey Way](https://twitter.com/jeffrey_way)


## References
1. http://stackoverflow.com/questions/3860112/multiple-github-accounts-on-the-same-computer
2. https://code.tutsplus.com/tutorials/quick-tip-how-to-work-with-github-and-multiple-accounts--net-22574
