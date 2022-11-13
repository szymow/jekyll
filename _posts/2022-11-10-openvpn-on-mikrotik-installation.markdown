---
layout: post
title:  "OpenVPN on Mikrotik Installation"
date:   2022-11-10 10:54:43 +0100
categories: installation openvpn
---

<a href="https://forum.pclab.pl/topic/1189947-jak-otworzy%C4%87-porty/">Otwieranie portu Toya</a>

Mikrotik Openvpn for remote access dial-in into a corporate network is one of the powerful ways to securely connect to a remote network and resolve one or two issues while having a time of your life in a location far away from the coperate network. At a time like this when most workers are working from home and organizations are battling to stay afloat, IT managers are looking for affordable yet secure VPN solutions for their telecommuters.

While the RouterOS is packed with many options for implementing site-to-site and remote access vpn, for example, IPSEC, GRE Tunneling, PPTP Tunneling, and L2TP, Mikrotik Openvpn is not only considered one of the most secured but also one of the easiest to setup and use on client devices.

In this post, I will be sharing the steps required to setpup Mikrotik Openvpn on routerOS as well as the installation and configuration of the Openvpn client application on an iOS device, eg. an iPhone.

The first step is to creat the certificates that will be used for the Mikrotik Openvpn setup. This involves the creation of three certificates on the Mikrotik router that will serve as your vpn gateway. This router must have a public IP address assigned to its internet-facing interface.

Creation of certificates
The three certificates that will be created are the Ca, server, and client certificates. Let’s create them below.

<img src="/assets/img/openvpn-mikrotik/cert_post1-1.png">
