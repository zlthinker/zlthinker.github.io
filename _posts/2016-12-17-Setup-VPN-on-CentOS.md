---
title: Steps to Setup VPN PPTP Client on CentOS7
updated: 2016-12-17 20:07
tags: [VPN, CentOS, OS]
category: Linux
---

1. Install PPTP:  **sudo yum install pptp pptp-setup**
2. Configuration:  **sudo pptpsetup --create config --server [server address] --username [username] --password [pwd] --encrypt**
    * This command will create a file named _config_ under _/etc/ppp/peers/_ with server info written inside.
    * This command will also write your username and password into _/etc/ppp/chap-secrets_
3. Register the _ppp_mppe_ kernel module: **sudo modprobe ppp_mppe**
4. Register the _nf_conntrack_pptp_ kernel module: **sudo modprobe nf_conntrack_pptp**
4. Connect to VPN PPTP: **sudo pppd call config**
    * It will establish PPTP VPN connection. You can type command **ip a I grep ppp** to find the connection name (e.g. _ppp1_). No return indicates connection failure.
    * If any error, you can look into **/var/log/messages** for log info
5. Check IP routing table info: **route -n**
6. Add Network Segment to current connection: **route add -net 192.168.1.0 netmask 255.255.255.0 dev ppp1** (_192.168.1.0_ and _ppp1_ are taken as example )
7. You can now ping the destination to check the access
8. Disconnect the VPN: **killall pppd**
