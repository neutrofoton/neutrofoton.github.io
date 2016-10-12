---
layout: post
title: "Ubuntu Server Does Not Recognize VirtualBox Adapter"
date: 2016-10-12 21:25:24 +0700
comments: true
categories: [linux,virtualbox]
---
As we know, VirtualBox provides several adapters that we can add and be used by OS guest.
At the time of writing this post, I have an OS X (host) and Ubuntu Server 16.04.1 LTS (guest). The needs are, I want to be able to remote my Ubuntu from OS X terminal. I also want my Ubuntu guest can connect to internet. I have configured two network adapters for the guest (NAT and host-only).

When I run
``` bash
$ ifconfig
```
The first/default adapter (NAT) <code>enp0s3</code> is recognized without any problems by the guest. But not the second one (host-only). To make the second adapter visible, I have to run  a command

``` bash
$ sudo dhclient
$ ifconfig
```

Now I got both adapters <code>enp0s3</code> (NAT) and <code>enp0s8</code> (host-only) visible on terminal. And now I can ping and run ssh from host (OS X) to guest (Ubuntu server).

In order to make it permanent, we need to edit <code>/etc/network/interfaces</code> with the following lines

```
# The second network interface
auto enp0s8
iface enp0s8 inet dhcp

```
Then restart the network service by running

``` bash
$ sudo /etc/init.d/networking restart
```

That's my note for now and thanks for reading.
