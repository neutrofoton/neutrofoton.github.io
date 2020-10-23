---
layout: post
title: "PPTP on Ubuntu"
date: 2020-10-23 12:31:07 +0700
comments: true
categories: [linux]
---


Previously, I have posted how to connect <a href="blog/2017/11/22/pptp-on-macos/"/>macOS to VPN server through PPTP protocol</a>. This post decribed how to do the same thing for Ubuntu. I used Ubuntu 18.04.3 LTS for testing.

The first step is installing PPTP client for Ubuntu.

``` bash
sudo apt-get -y install pptp-linux
```

Create VPN configuration file
``` bash
sudo nano /etc/ppp/peers/myPPTP
```

paste the following script

``` bash
pty "pptp IP_ADDRESS --nolaunchpppd --debug"
name USERNAME
password PASSWORD
remotename PPTP
require-mppe-128
require-mschap-v2
refuse-eap
refuse-pap
refuse-chap
refuse-mschap
noauth
debug
persist
maxfail 0
defaultroute
replacedefaultroute
usepeerdns
```

Then save and exit the file. Before executing VPN connection, change the file security attribute.

``` bash
chmod 600  /etc/ppp/peers/myPPTP
```

To connect to the VPN server, type the following command.
``` bash
pon myPPTP
```

To disconnect from the VPN server, run the following command.
``` bash
poff myPPTP
```

If you fail connect to the VPN server, please check the firewall configuration.

# References
1. https://support.strongvpn.com/hc/en-us/articles/360003513553-PPTP-Setup-Debian-Ubuntu-Command-Line
2. https://www.networkinghowtos.com/howto/connect-to-a-pptp-vpn-server-from-ubuntu-linux/

