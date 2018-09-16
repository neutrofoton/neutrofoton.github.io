---
layout: post
title: "PPTP on macOS"
date: 2017-11-22 06:59:38 +0700
comments: true
categories: [macos]
---

One day I need to connect my macOS to a network of client of the company I work for via [Point-to-Point Tunneling Protocol (PPTP)](https://en.wikipedia.org/wiki/Point-to-Point_Tunneling_Protocol) VPN. Unfortunately [Apple removed PPTP support on macOS Sierra](https://support.apple.com/en-us/HT206844), so I had to find an alternative for that. Some of them I found are third parties application that need a one time buying or annual subscription. In fact, Apple just remove the user interface option for PPTP VPN, meanwhile the libraries of it are still available on Sierre.

Since the libraries of PPTP are still available on Sierra, theoritically we should be able to call the libraries via terminal. Finally I found 3 blogs that write about PPTP protocol on macos and I put them in a reference section in this blog. Basically the three of them use the same technique that's write a script contains configuration of PPTP that's put in <code>/etc/ppp/peers/</code> and call it via <code>pppd</code> command via terminal.


First of all create a file called <code>/etc/ppp/peers/pptpvpn-client1</code>

``` bash
$ sudo /etc/ppp/peers/pptpvpn-client1

```

Fill the <code>pptpvpn-client1</code> that contains configuration that pppd daemon will refer to connect.

``` bash /etc/ppp/peers/pptpvpn-client1

plugin PPTP.ppp
noauth
# logfile /tmp/ppp.log
remoteaddress "xxx.xxx.xxx.xxx"
user "xxxxxx"
password "xxxxxxxx"
redialcount 1
redialtimer 5
idle 1800
# mru 1368
# mtu 1368
receive-all
novj 0:0
ipcp-accept-local
ipcp-accept-remote
# noauth
refuse-eap
refuse-pap
refuse-chap-md5
hide-password
mppe-stateless
mppe-128
# require-mppe-128
looplocal
nodetach
# ms-dns 8.8.8.8
usepeerdns
# ipparam gwvpn
defaultroute
debug

```

Then open terminal and call
``` bash
$ sudo pppd call pptpvpn-client1
```

If you cannot connect with the configuration code I use, you can check the error messages displayed in terminal. May be some configuration items do not match with the vpn server setting you connect to.

If the you got no any error messages and connection established with your VPN network you can open a new tab on the terminal and try to ping to an ip address in the VPN local network.

## References
1. https://smallhacks.wordpress.com/2016/12/20/pptp-on-osx-sierra/
2. https://malucelli.net/2017/05/16/pptp-vpn-on-macos-sierra/
3. https://www.cts-llc.net/2017/02/21/pptp-on-osx-just-one-last-time.html
