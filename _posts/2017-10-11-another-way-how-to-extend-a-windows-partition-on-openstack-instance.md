---
id: 698
title: Another way How to Extend a Windows Partition on OpenStack Instance
date: 2017-10-11T19:48:18+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=698
permalink: /another-way-how-to-extend-a-windows-partition-on-openstack-instance/
categories:
  - Linux
  - OpenStack
tags:
  - KVM
  - OpenSource
  - OpenStack
  - Server
  - Windows
---
**\### Environment**  
&#8211; OpenStack Newton (KVM)  
&#8211; Instance OS Windows Server 2012 R2  
&#8211; Disk = 1TB [C:] 200GB [D:] 800GB  
&#8211; Add Disk 1TB to [D:] Partition

**\### Compute OpenStack**

<pre>cd /var/lib/nova/instance/a1b2c3e4f5gxxxxxx
qemu-img create -f qcow2 disk-new.qcow2 2040G
virt-resize disk disk-new.qcow2 --expand /dev/sda2</pre>

<pre>[root@openstack a1b2c3e4f5gxxxxxx]# virt-resize disk disk-new.qcow2 --expand /dev/sda2</pre>

_Output:_

<pre>[   0.0] Examining disk
**********

Summary of changes:

/dev/sda1: This partition will be left alone.

/dev/sda2: This partition will be resized from 824.0G to 1840.0G.

**********
[   3.8] Setting up initial partition table on disk-new.qcow2
[   4.0] Copying /dev/sda1
 100% ⟦▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒⟧ 00:00
[ 865.4] Copying /dev/sda2
100% ⟦▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒⟧ 00:00
Resize operation completed with no errors.  Before deleting the old disk,
carefully check that the resized disk boots and works correctly.</pre>

<pre>[root@openstack a1b2c3e4f5gxxxxxx]# cp -v disk-new.qcow2 disk</pre>

**\### Windows Powel Shell**

<pre>C:\>chkdsk D:</pre>

<pre>C:\>diskpart </pre>

<pre>DISKPART> list volume </pre>

<pre>DISKPART> select volume 1 </pre>

<pre>DISKPART> extend filesystem

DiskPart successfully extended the file system on the volume.</pre>

<pre>DISKPART> list volume

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C   WINSRV2012R  NTFS   Partition    200 GB  Healthy    System
  Volume 1     D   DATA         NTFS   Partition   1839 GB  Healthy
</pre>

<pre>DISKPART> exit</pre>

* * *

**Refrensi:** <a href="https://support.microsoft.com/en-us/help/832316/the-partition-size-is-extended-but-the-file-system-remains-the-origina" rel="noopener" target="_blank">https://support.microsoft.com/en-us/help/832316/the-partition-size-is-extended-but-the-file-system-remains-the-origina</a>