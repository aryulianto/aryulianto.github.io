---
id: 1186
title: Get Back OpenStack Instances State after Host Reboot
date: 2019-02-18T13:05:53+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=1186
permalink: /get-back-openstack-instances-state-after-host-reboot/
categories:
  - Linux
  - OpenStack
tags:
  - Instances
  - nova
  - OpenStack
---
Pernah gak ngalamin node Compute _reboot_? baik disengaja maupun tidak, lalu semua Instances OpenStack di dalamnya statusnya berubah jadi **SHUTOFF** dan kita mesti start atau sesuaikan _state_-nya secara manual 😀 mungkin masalah yang kalian alami sama, yaitu belum mengaktifkan opsi ini di konfigurasi Nova

<pre># This option specifies whether to start guests that were running before the
# host rebooted. It ensures that all of the instances on a Nova compute node
# resume their state each time the compute node boots or restarts.
# (boolean value)
#resume_guests_state_on_host_boot=false
</pre>

Lalu bagaimana agar state Instances kembali menjadi sediakala sebelum host Compute direboot, cukup aktifkan opsi ini

<pre class="">vim /etc/nova/nova.conf

resume_guests_state_on_host_boot=true</pre>

Setelah itu muat ulang services Nova

<pre>systemctl restart openstack-nova-compute
systemctl sttus openstack-nova-compute</pre>

Uji coba, cek uptime dan list Instances OpenStack untuk mengetahui state awal

<pre class="">[root@openstack]# uptime; openstack server list
 12:50:57 up 35 min,  1 user,  load average: 0.55, 0.90, 1.10
+--------------------------------------+----------+-----------+----------------------+--------------------------+---------+
| ID                                   | Name     | Status    | Networks             | Image                    | Flavor  |
+--------------------------------------+----------+-----------+----------------------+--------------------------+---------+
| fdd0bb76-3581-44b3-a275-49198cab4e69 | cirros-3 | SHUTOFF   | net-ext=172.18.1.101 | cirros-0.4.0-x86_64-disk | m1.tiny |
| 396d50fd-c919-4a89-a4f6-99cb7ea82937 | cirros-1 | ACTIVE    | net-ext=172.18.1.120 | cirros-0.4.0-x86_64-disk | m1.tiny |
| c00c4b70-168a-4e91-a77f-fc701344fe35 | cirros-2 | SUSPENDED | net-ext=172.18.1.125 | cirros-0.4.0-x86_64-disk | m1.tiny |
+--------------------------------------+----------+-----------+----------------------+--------------------------+---------+
</pre>

Selanjutnya, coba _reboot_ node Compute dan cek Instances kalian kembali

<pre class="">[root@openstack]# uptime; openstack server list
 12:59:31 up 3 min,  1 user,  load average: 6.64, 5.49, 2.35
+--------------------------------------+----------+-----------+----------------------+--------------------------+---------+
| ID                                   | Name     | Status    | Networks             | Image                    | Flavor  |
+--------------------------------------+----------+-----------+----------------------+--------------------------+---------+
| fdd0bb76-3581-44b3-a275-49198cab4e69 | cirros-3 | SHUTOFF   | net-ext=172.18.1.101 | cirros-0.4.0-x86_64-disk | m1.tiny |
| 396d50fd-c919-4a89-a4f6-99cb7ea82937 | cirros-1 | ACTIVE    | net-ext=172.18.1.120 | cirros-0.4.0-x86_64-disk | m1.tiny |
| c00c4b70-168a-4e91-a77f-fc701344fe35 | cirros-2 | SUSPENDED | net-ext=172.18.1.125 | cirros-0.4.0-x86_64-disk | m1.tiny |
+--------------------------------------+----------+-----------+----------------------+--------------------------+---------+
</pre>

**Sekian dan Terima kasih**