---
id: 1019
title: Failover using VRRP Keepalived
date: 2018-07-24T11:33:52+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=1019
permalink: /failover-using-vrrp-keepalived/
categories:
  - Linux
tags:
  - Failover
  - HAProxy
  - Keepalived
  - VRRP
---
**Requirements:**

  * 2 Server atau lebih dalam 1 jaringan yang sama
  * 1 Virtual IP (VIP)

Disini saya menggunakan 3 Server Ubuntu 16.04 dan <a href="http://www.haproxy.org/" rel="noopener" target="_blank">HAProxy</a> dibelakangnya:  
\`node1 = 10.0.0.5  
node2 = 10.0.0.6  
node3 = 10.0.0.7  
VIP = 10.0.0.200\`

Langsung saja berikut cara _Failover_ menggunakan VRRP _(Virtual Router Redundancy Protocol)_ sebagai berikut

Pasang paket Keepalived disemua node

<pre>apt install keepalived -y</pre>

Buat Konfigurasi Keepalived disemua node

<pre>vim /etc/keepalived/keepalived.conf</pre>

<pre>vrrp_script haproxy-check {
    script "killall -0 haproxy"
    interval 2
    weight 20
}
 
vrrp_instance haproxy-vip {
    state MASTER
    priority 101
    interface ens3
    virtual_router_id 51
    advert_int 3
 
    unicast_src_ip 10.0.0.5
    unicast_peer {
        10.0.0.6
        10.0.0.7
    }
 
    virtual_ipaddress {
        10.0.0.200
    }
 
    track_script {
        haproxy-check weight 20
    }</pre>

Sesuaikan bagian:  
\`interface\` (interface yang digunakan untuk failover)  
\`priority\` (prioritas masing-masing node)  
\`unicast\_src\_ip\` (IP node tersebut)  
\`unicast_peer\` (IP node lainnya) 

Muat ulang servis Keepalived

<pre>systemctl enable keepalived
systemctl restart keepalived
systemctl status keepalived</pre>

Verifikasi disemua node:

<pre>root@node3:~# ip address 
1: lo: &lt;LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens3: &lt;BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 52:54:00:52:a1:72 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.7/24 brd 10.0.0.255 scope global ens3
       valid_lft forever preferred_lft forever
    inet 10.0.0.200/32 scope global ens3
       valid_lft forever preferred_lft forever
    inet6 fe80::5054:ff:fe52:a172/64 scope link</pre>

Test Failover  
~ Matikan node yang saat ini menempel VIP  
~ Pantau konfigurasi IP di node lainnya dengan perintah 

<pre>watch -n1 ip address</pre>

**Sekian.**