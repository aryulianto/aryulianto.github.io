---
id: 936
title: Force Delete Cinder Volume OpenStack
date: 2018-03-30T07:46:35+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=936
permalink: /force-delete-cinder-volume-openstack/
categories:
  - Linux
  - OpenStack
tags:
  - Cinder
  - OpenStack
---
<pre>[root@controller~]# cinder list</pre>

<pre>[root@controller~]# cinder reset-state --state available $VOLUME_UUID</pre>

<pre>[root@controller~]# cinder reset-state --attach-status detached $VOLUME_UUID</pre>

<pre>[root@controller~]# cinder delete $VOLUME_UUID</pre>

<pre>[root@controller~]# mysql -u root -p</pre>

<pre>mysql> use cinder;</pre>

<pre>mysql>update volumes set attach_status='detached',status='available' where id ='$VOLUME_UUID';</pre>

<pre>[root@controller~]# cinder delete $VOLUME_UUID</pre>

<pre>mysql>update volumes set deleted=1,status='deleted',deleted_at=now(),updated_at=now() where deleted=0 and id='$VOLUME_UUID';</pre>