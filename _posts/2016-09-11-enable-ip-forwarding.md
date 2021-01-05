---
id: 465
title: Enable IP Forwarding
date: 2016-09-11T13:46:58+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=465
permalink: /enable-ip-forwarding/
categories:
  - Linux
tags:
  - forwarding
  - ip
---
Check if IP Forwarding is enabled ?

<pre>cat /proc/sys/net/ipv4/ip_forward</pre>

if you see the value 0 this was _Disable_.  
Edit from 0 to 1 for _Enable_ with the command below

<pre>echo 1 > /proc/sys/net/ipv4/ip_forward</pre>

Load

<pre>sysctl -f</pre>

Restart the network service

<pre>service network restart</pre>