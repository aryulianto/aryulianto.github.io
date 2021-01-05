---
id: 1039
title: Enable Soft Delete to Recover Deleted Instances OpenStack
date: 2018-10-28T12:42:50+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=1039
permalink: /enable-soft-delete-to-recover-deleted-instances-openstack/
categories:
  - Linux
  - OpenStack
tags:
  - Delete
  - Instance
  - nova
  - OpenStack
---
Ubah konfigurasi Nova di node semua Controller dan semua node Compute

<pre>vim /etc/nova/nova.conf</pre>

Cari nilai _**reclaim\_instance\_interval**_

<pre>reclaim_instance_interval=0</pre>

<pre># * Any positive integer(in seconds) greater than 0 will enable
#   this option.
# * Any value &lt;=0 will disable the option.
#  (integer value)</pre>

Ubah nilainya sesuai yang diinginkan misal ingin menahan Instance agar tidak dihapus secara permanent selama 1 hari (24 jam x 3600 = 86400 detik) berarti isikan

<pre>reclaim_instance_interval=86400</pre>

Muat ulang service nova-api di Node Controller

<pre>systemctl restart openstack-nova-api.service
systemctl status openstack-nova-api.service</pre>

Muat ulang service nova-compute di Node Compute

<pre>systemctl restart openstack-nova-compute.service 
systemctl status openstack-nova-compute.service</pre>

List Instances

<pre>[root@openstack ~(keystone_admin)]# openstack server list
+--------------------------------------+-------------+--------+------------------+-------------------------------------------------------+-----------+
| ID                                   | Name        | Status | Networks         | Image                                                 | Flavor    |
+--------------------------------------+-------------+--------+------------------+-------------------------------------------------------+-----------+
| 2cffc5b3-feb3-40a6-b6af-62cdde03d69c | soft-delete | ACTIVE | extnet=10.1.1.49 | cirros-0.4.0-x86_64-disk.img                          | ns.1-1-1  |
| 281a9a9b-0fe7-48d7-93e0-3dae7c9db695 | instance-01 | ACTIVE | extnet=10.1.1.2  | ubuntu-16.04-server-cloudimg-amd64-disk1-20180306.img | ns.2-2-20 |
+--------------------------------------+-------------+--------+------------------+-------------------------------------------------------+-----------+</pre>

Ujicoba hapus Instance

<pre>[root@openstack ~(keystone_admin)]# openstack server delete 2cffc5b3-feb3-40a6-b6af-62cdde03d69c

[root@openstack ~(keystone_admin)]# openstack server list
+--------------------------------------+-------------+--------+-----------------+-------------------------------------------------------+-----------+
| ID                                   | Name        | Status | Networks        | Image                                                 | Flavor    |
+--------------------------------------+-------------+--------+-----------------+-------------------------------------------------------+-----------+
| 281a9a9b-0fe7-48d7-93e0-3dae7c9db695 | instance-01 | ACTIVE | extnet=10.1.1.2 | ubuntu-16.04-server-cloudimg-amd64-disk1-20180306.img | ns.2-2-20 |
+--------------------------------------+-------------+--------+-----------------+-------------------------------------------------------+-----------+</pre>

Cek List instance yang sudah di Hapus, jika kita aktifkan Soft Delete maka statusnya akan menjadi **SOFT_DELETE** sampai batas waktu yang sudah kita tentukan akan berubah menjadi **DELETED**

<pre>[root@openstack ~(keystone_admin)]# openstack server list --deleted
+--------------------------------------+------------------+--------------+------------------+-----------------------------------------+-----------+
| ID                                   | Name             | Status       | Networks         | Image                                   | Flavor    |
+--------------------------------------+------------------+--------------+------------------+-----------------------------------------+-----------+
| 2cffc5b3-feb3-40a6-b6af-62cdde03d69c | soft-delete      | SOFT_DELETED | extnet=10.1.1.49 | cirros-0.4.0-x86_64-disk.img            | ns.1-1-1  |
| 239c18af-3ffc-41aa-9a47-d07e9af4ac52 | test-centos      | DELETED      |                  | CentOS-7-x86_64-GenericCloud-1802.qcow2 | ns.2-4-20 |
| 83448c2a-66fe-4864-8296-253ed4bcb16f | cirros-test      | DELETED      |                  | cirros-0.4.0-x86_64-disk.img            | ns.1-1-1  |
+--------------------------------------+------------------+--------------+------------------+-----------------------------------------+-----------+</pre>

Ujicoba Restore Instance

<pre>[root@openstack ~(keystone_admin)]# openstack server restore 2cffc5b3-feb3-40a6-b6af-62cdde03d69c

[root@openstack ~(keystone_admin)]# openstack server list
+--------------------------------------+-------------+--------+------------------+-------------------------------------------------------+-----------+
| ID                                   | Name        | Status | Networks         | Image                                                 | Flavor    |
+--------------------------------------+-------------+--------+------------------+-------------------------------------------------------+-----------+
| 2cffc5b3-feb3-40a6-b6af-62cdde03d69c | soft-delete | ACTIVE | extnet=10.1.1.49 | cirros-0.4.0-x86_64-disk.img                          | ns.1-1-1  |
| 281a9a9b-0fe7-48d7-93e0-3dae7c9db695 | instance-01 | ACTIVE | extnet=10.1.1.2  | ubuntu-16.04-server-cloudimg-amd64-disk1-20180306.img | ns.2-2-20 |
+--------------------------------------+-------------+--------+------------------+-------------------------------------------------------+-----------+</pre>

Lalu bagaimana jika kita ingin segera memakai resource kita tanpa menunggu 1 hari? bisa gunakan perintah 

<pre>[root@openstack ~(keystone_admin)]# nova force-delete 2cffc5b3-feb3-40a6-b6af-62cdde03d69c

[root@openstack ~(keystone_admin)]# openstack server list --deleted
+--------------------------------------+------------------+---------+----------+-----------------------------------------+-----------+
| ID                                   | Name             | Status  | Networks | Image                                   | Flavor    |
+--------------------------------------+------------------+---------+----------+-----------------------------------------+-----------+
| 2cffc5b3-feb3-40a6-b6af-62cdde03d69c | soft-delete      | DELETED |          | cirros-0.4.0-x86_64-disk.img            | ns.1-1-1  |
| 239c18af-3ffc-41aa-9a47-d07e9af4ac52 | test-centos      | DELETED |          | CentOS-7-x86_64-GenericCloud-1802.qcow2 | ns.2-4-20 |
| 83448c2a-66fe-4864-8296-253ed4bcb16f | cirros-test      | DELETED |          | cirros-0.4.0-x86_64-disk.img            | ns.1-1-1  |
+--------------------------------------+------------------+---------+----------+-----------------------------------------+-----------+</pre>

**Sekian dan Terima kasih!**