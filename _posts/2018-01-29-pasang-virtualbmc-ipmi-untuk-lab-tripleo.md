---
id: 848
title: Pasang VirtualBMC (IPMI) untuk Lab TripleO
date: 2018-01-29T13:25:26+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=848
permalink: /pasang-virtualbmc-ipmi-untuk-lab-tripleo/
categories:
  - Linux
  - OpenStack
tags:
  - IPMI
  - OpenStack
  - openSUSE
  - TripleO
  - VBMC
---
Sebenarnya ada beberapa cara untuk memasang VirtualBMC cara termudah ialah menggunakan _pip install_ atau bisa juga pasang manual langsung dari sumbernya di <a href="https://github.com/openstack/virtualbmc" target="_blank" rel="noopener noreferrer">git</a>.

**Hypervisor**  
openSUSE 42.3 + KVM (10.2.2.1)

Pasang _python-pip_ dan _virtualbmc_

<pre class="">zypper in python-pip gcc python-devel libvirt-devel
pip install --upgrade pip
pip search virtualbmc
pip install virtualbmc</pre>

Periksa nama VM overcloud yang akan di kontrol oleh VM undercloud dengan perintah IPMI

<pre>host:~ # virsh list --all |grep ooo
 67    ooo-uc                         running
 -     ooo-srv0                       shut off
 -     ooo-srv1                       shut off
 -     ooo-srv2                       shut off
 -     ooo-srv3                       shut off
</pre>

Tambahkan vbmc ke VM, defaultnya IPMI menggunakan port 623, kalian juga bisa menggunakan opsi _-p/&#8211;port_ jika ingin menggunakan port yang berbeda

<pre class="">vbmc add ooo-srv0 --username admin --password rahasia --port 623
vbmc add ooo-srv1 --username admin --password rahasia --port 624
vbmc add ooo-srv2 --username admin --password rahasia --port 625
vbmc add ooo-srv3 --username admin --password rahasia --port 626</pre>

Jalankan vbmc di host

<pre class="">vbmc start ooo-srv0
vbmc start ooo-srv1
vbmc start ooo-srv2
vbmc start ooo-srv3</pre>

Verifikasi, pastikan statusnya _running_

<pre class="">host:~ # vbmc list
+-------------+---------+---------+------+
| Domain name |  Status | Address | Port |
+-------------+---------+---------+------+
|   ooo-srv0  | running |    ::   | 623  |
|   ooo-srv1  | running |    ::   | 624  |
|   ooo-srv2  | running |    ::   | 625  |
|   ooo-srv3  | running |    ::   | 626  |
+-------------+---------+---------+------+
</pre>

**VM Undercloud**  
Cek konektifias ke node overcloud menggunakan perintah ipmitool:

<pre class="">[stack@undercloud ~]$ ipmitool -I lanplus -U admin -P rahasia -H [ip-host] power status -p 623
Chassis Power is off</pre>

**TripleO**  
Register node overcloud di UI TripleO

[<img class="alignleft size-large wp-image-888" src="http://blog.aryulianto.com/wp-content/uploads/2018/01/Selection_011-1024x427.png" alt="" width="730" height="304" srcset="https://blog.aryulianto.com/wp-content/uploads/2018/01/Selection_011-1024x427.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2018/01/Selection_011-300x125.png 300w, https://blog.aryulianto.com/wp-content/uploads/2018/01/Selection_011-768x320.png 768w, https://blog.aryulianto.com/wp-content/uploads/2018/01/Selection_011.png 1033w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2018/01/Selection_011.png)  
Lalu masukan _NIC MAC Addresses_ dibagian bawah dan klik _Register Nodes._

**Troubleshooting:**  
1. Error: Unable to establish IPMI v2 / RMCP+ session  
Pastikan port yang dipakai di vbmc di allow di firewall

<pre class="">sudo firewall-cmd --permanent --zone=public --add-port=623-626/udp
sudo firewall-cmd --reload</pre>

2. Exception TypeError: &#8220;&#8216;NoneType&#8217; object is not callable&#8221; in ignored  
Error ini bisa diabaikan saja.

**Update: Install VirtualBMC di CentOS 8**

<pre class="">yum update -y
yum config-manager --set-enabled PowerTools
yum install centos-release-openstack-train
yum install python3-virtualbmc.noarch
logout
</pre>

* * *

**Refrensi: ** <a href="http://tripleo.org/environments/virtualbmc.html" target="_blank" rel="noopener noreferrer">Virtual BMC TripleO Documentation</a>