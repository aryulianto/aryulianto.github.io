---
id: 838
title: Konfigurasi VLAN Tagging dengan IP Command
date: 2017-12-10T10:55:07+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=838
permalink: /konfigurasi-vlan-tagging-dengan-ip-command/
categories:
  - Linux
tags:
  - ip
  - openSUSE
  - vlan
---
Membuat VLAN tagging di interface virbr1 dengan nama dan ID VLAN 100

<pre>ip link add link virbr1 name virbr1.100 type vlan id 100
ip link set dev virbr1.100 up</pre>

Verifikasi

<pre>ip -d link show virbr1.100

10: virbr1.100@virbr1: &lt;BROADCAST,MULTICAST&gt; mtu 1500 qdisc noop state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:50:86:8d brd ff:ff:ff:ff:ff:ff promiscuity 0 
    vlan protocol 802.1Q id 100  addrgenmode eui64</pre>

Menambahkan IP ke interface VLAN

<pre>ip add add 10.10.100.1/24 dev virbr1.100</pre>

Verifikasi

<pre>ip -d a show virbr1.100

10: virbr1.100@virbr1: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 52:54:00:50:86:8d brd ff:ff:ff:ff:ff:ff promiscuity 0 
    vlan protocol 802.1Q id 100  
    inet 10.10.100.1/24 scope global virbr1.100
       valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe50:868d/64 scope link 
       valid_lft forever preferred_lft forever
</pre>

Hapus VLAN

<pre>ip link delete virbr1.100</pre>

Catatan: _VLAN yang dibuat dengan ip command akan hilang jika sistem shutdown atau restart, jika ingin membuatnya permanen gunakan <a href="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-Configure_802_1Q_VLAN_Tagging_Using_the_Command_Line#sec-Setting_Up_802.1Q_VLAN_Tagging_Using_ifcfg_Files" rel="noopener" target="_blank">ifcfg</a> files_

* * *

**Refrensi:** <a href="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-configure_802_1q_vlan_tagging_using_the_command_line#sec-Configure_802_1Q_VLAN_Tagging_ip_Commands" target="_blank" rel="noopener">7.4. Configure 802.1Q VLAN Tagging Using the Command Line</a>