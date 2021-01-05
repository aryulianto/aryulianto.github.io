---
id: 591
title: Proxmox Bind Mount – Mount Storage dari Host ke container LXC
date: 2016-11-15T14:25:42+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=591
permalink: /proxmox-bind-mount-mount-storage-host-di-lxc-container/
categories:
  - Linux
tags:
  - Linux
  - Proxmox
  - Virtualisasi
---
Sebelum mulai pastikan kedua direktori di Host dan di lokasi target Container sudah dibuat. Sebagai contoh saya ingin me-mount direktori  
**/mnt/share-storage/** dari Host ke dalam Container direktori **/mnt/data/**  
Mesin Proxmox yang saya gunakan versi: **<a href="https://www.proxmox.com/en/downloads" target="_blank">Proxmox Virtual Environment 4.3</a>**

1. Cek ID container

<pre>root@host:~# lxc-ls --fancy
NAME STATE   AUTOSTART GROUPS IPV4                 
311  RUNNING 0         -      10.0.0.211, -    
312  RUNNING 0         -      10.0.0.212, -    
313  RUNNING 0         -      10.0.0.213, -</pre>

2. Edit berkas konfigurasi container LXC

<pre>root@host:~# vim /etc/pve/lxc/313.conf</pre>

3. Tambahkan baris berikut

<pre>mp0:<strong>[SOURCE-HOST]</strong>,mp=<strong>[TARGET-CONTAINER]</strong>
mp0:<strong>/mnt/share-storage/</strong>,mp=<strong>/mnt/data/</strong></pre>

4. Kemudian muat ulang container

<pre>root@host:~# lxc-stop -n 313 
root@host:~# lxc-start -n 313</pre>

5. Verifikasi hasil bind mount didalam container

<pre>root@host:~# lxc-attach -n 313 

root@ct313:~# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/loop2      148G  867M  140G   1% /
<strong>/dev/sdb1       1.1T   71M  1.1T   1% /mnt/data</strong>
none            492K     0  492K   0% /dev
tmpfs            32G     0   32G   0% /dev/shm
tmpfs            32G  8.2M   32G   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs            32G     0   32G   0% /sys/fs/cgroup</pre>

6. Jika Anda memiliki beberapa path yang ingin di-mount maka tinggal tambahkan saja dibawah baris mp0 jadi mp1, mp2, dst

<pre>mp0:<strong>/mnt/share-storage/</strong>,mp=<strong>/mnt/data/</strong>
mp1:/mnt/pve/share-storage1,mp=/mnt/data1
mp2:/mnt/pve/share-storage2,mp=/mnt/data2
...</pre>

**~Sekian dan Semoga bermanfaat. 😀**