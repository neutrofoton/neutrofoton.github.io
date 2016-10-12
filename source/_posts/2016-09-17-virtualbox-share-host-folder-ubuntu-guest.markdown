---
layout: post
title: "VirtualBox Share Host Folder Ubuntu Guest"
date: 2016-09-17 10:41:13 +0700
comments: true
categories: [linux,virtualbox]
---
<a href="https://www.virtualbox.org/">VM VirtualBox</a> is one of free popular <a href="https://en.wikipedia.org/wiki/Hypervisor">hypervisor</a> for x86 computer from Oracle. I use it as part of my development environment, since occasionally I need several different Operating Systems for development and software testing  or deployment testing. When we work with VirtualBox we often need to share folder between host and guest operating system.

Before sharing folder, ensure we have installed Guest Addition that shipped with VirtualBox. To do that :

 1. Go to  Device menu > Insert Guest CD Addition Images.
 2. If it automatically mounting and has popup dialog, just hit run otherwise we may have to manually mounting it.

 I usually restart my OS guest after installing this VirtualBox Guest Addition.

The steps are :


- Go to Devices menu > Shared Folders > Shared Folder Settings
- Select folder path of Host to be shared to Guest OS
- Checked Make Permanent option if we want to make it permanent

   > On my sample, I shared folder from OS X Host /Volumes/MacintoshHDData/share

   Till this step is enough for Windows guest. On Ubuntu guest, it need additional steps to make the shared folder accessible in guest.

- When we shared a Host's folder on Ubuntu guest, VirtualBox should create a folder on <code>/Media</code> just like the shared folder name with <code>sf_</code> prefix. In my case <code>/Media/sf_share</code>. If the folder does not exist, open terminal, create directory <code>/Media/sf_share</code> and give folder name

  ```
  mkdir /Media/sf_share
  ```

- VirtualBox with Ubuntu operating system guest, it added a group called <code>vboxsf</code>. Before we can access the shared folder we have to be a member of the group.

  ``` bash
  sudo add user [username] vboxsf
  ```

   To verify what groups are username belongs to,

  ``` bash
  id [username]
  ```

- Mount the shared folder

  ``` bash
  sudo mount -t vboxsf share /media/sf_share
  ```

Now we should be able to the shared folder via Nautilus (File Manager). To make it automatic mounting, ensure we checked option automatic mounting on shared folder setting dialog.

## References
1. http://askubuntu.com/questions/456400/why-cant-i-access-a-shared-folder-from-within-my-virtualbox-machine
2. http://www.howtogeek.com/187703/how-to-access-folders-on-your-host-machine-from-an-ubuntu-virtual-machine-in-virtualbox/
