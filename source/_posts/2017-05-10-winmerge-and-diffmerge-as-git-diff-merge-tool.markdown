---
layout: post
title: "WinMerge and DiffMerge as git diff merge tool"
date: 2017-05-10 10:01:23 +0700
comments: true
categories: [git, macos, windows]
---

{% img left /images/logo/git.png %}
On software development while working with source control, it's inevitable sometime we get our code conflicts with other, since we work in a team. There are many diff and merge tools out there and some of them can be integrated with with Git. In this post I just want to note what I did in my development machine (Windows 7 and macOS Sierra)


## DiffMerge on macOS
For my macOS development machine I use [DiffMerge](http://www.sourcegear.com/diffmerge/). Actually DiffMerge is not only available for macOS, but also for Windows and Linux. So we can use it as Git diff merge tool as well on Windows and Linux. To configure Git to use DiffMerge can be done by running the following command via terminal.

``` bash

$ git config --global mergetool.prompt false
$ git config --global mergetool.keepBackup false
$ git config --global mergetool.keepTemporaries false

$ git config --global diff.tool diffmerge
$ git config --global difftool.diffmerge.cmd 'diffmerge "$LOCAL" "$REMOTE"'

$ git config --global merge.tool diffmerge
$ git config --global mergetool.diffmerge.cmd 'diffmerge --merge --result="$MERGED" "$LOCAL" "$(if test -f "$BASE"; then echo "$BASE"; else echo "$LOCAL"; fi)" "$REMOTE"'
$ git config --global mergetool.diffmerge.trustExitCode true
```

The command will add the following config code in global <code>.gitconfig</code>

```
[mergetool]
  prompt = false
  keepBackup = false
  keepTemporaries = false

[diff]
	tool = diffmerge

[difftool "diffmerge"]
	cmd = diffmerge \"$LOCAL\" \"$REMOTE\"

[merge]
	tool = diffmerge

[mergetool "diffmerge"]
	cmd = "diffmerge --merge --result=\"$MERGED\" \"$LOCAL\" \"$(if test -f \"$BASE\"; then echo \"$BASE\"; else echo \"$LOCAL\"; fi)\" \"$REMOTE\""
	trustExitCode = true
```
We can also directly edit the <code>.gitconfig</code> and manually add the config code.

## WinMerge 2.x on Windows

[WinMerge](http://winmerge.org/) is an open source differencing and merging tool for Windows. It can compare both folders and files, presenting differences in a visual text format that is easy to understand and handle.
At the time of writing this blog post, WinMerge 3 is still in progress of development and no release yet. WinMerge 3 will be modern compare/synchronization tool. It will be based on Qt library and cross-platform. You can use the same tool in Windows and in Linux.
So for now and so on in this blog post, WinMerge term means WinMerge 2.x.

After installing WinMerge, to configure it as diff and merge tool of Git is by adding /editing the following config setting to <code>C:\Users\\{UserName}\\.gitconfig</code>

``` bash

[mergetool]
  prompt = false
  keepBackup = false
  keepTemporaries = false

[merge]
  tool = winmerge

[mergetool "winmerge"]
  name = WinMerge
  trustExitCode = true
  cmd = "/c/Program\\ Files\\ \\(x86\\)/WinMerge/WinMergeU.exe" -u -e -dl \"Local\" -dr \"Remote\" $LOCAL $REMOTE $MERGED

[diff]
  tool = winmerge

[difftool "winmerge"]
  name = WinMerge
  trustExitCode = true
  cmd = "/c/Program\\ Files\\ \\(x86\\)/WinMerge/WinMergeU.exe" -u -e $LOCAL $REMOTE

```

The config above also can be configured by Git bash shell with <code>--global</code> parameter instead of manual edit via text editor.

Now, whenever you want it to launch diffs just use difftool[1]:

``` bash
# diff the local file.m against the checked-in version
$ git difftool file.m

# diff the local file.m against the version in some-feature-branch
$ git difftool some-feature-branch file.m

# diff the file.m from the Build-54 tag to the Build-55 tag
$ git difftool Build-54..Build-55 file.m

```

To resolve merge conflicts
```
$ git mergetool
```

## References
1. http://twobitlabs.com/2011/08/install-diffmerge-git-mac-os-x/
2. http://winmerge.org/
