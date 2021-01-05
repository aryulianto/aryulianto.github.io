---
id: 962
title: Grep Volume ID OpenStack
date: 2018-05-31T13:47:58+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=962
permalink: /grep-volume-id-openstack/
categories:
  - Linux
  - OpenStack
tags:
  - Cinder
  - OpenSource
  - OpenStack
  - Volume
---
List Volume ID di Satu Project

<pre>openstack volume list -f value --project user | awk '{print $1}'</pre>

List Volume ID di Banyak Project

<pre>while read X; do openstack volume list -f value --project $X; done &lt; file.csv | awk '{print $1}'</pre>

Sekian. :)