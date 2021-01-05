---
id: 977
title: Attach Multiple Network Interfaces to Instance OpenStack
date: 2018-06-10T21:43:28+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=977
permalink: /attach-multiple-network-interfaces-to-instance-openstack/
categories:
  - Linux
  - OpenStack
tags:
  - Instance
  - neutron
  - NIC
  - OpenStack
---
Dalam beberapa kasus kalian mungkin membutuhkan 2 _Internal Network_ dalam satu Instance, dan itu memang sudah didukung dengan baik oleh <a href="http://www.openstack.org" target="_blank" rel="noopener">OpenStack</a> bahkan saat Instance sudah berjalan kita bisa dapat secara _live_ menambahkan _Interface_ baru 😀

Tapi jika ada lebih dari satu _Interface_ dalam satu Instance, _<a href="http://cloudinit.readthedocs.io/en/latest/" target="_blank" rel="noopener">cloud-init</a>/images_ tidak secara otomatis mengkonfigurasi seluruh _Interface_. Maka dari itu kita perlu melakukan langkah tambahan agar seluruh Interface dapat berjalan dengan semestinya:

**CentOS 7**  
Cara Ke-1:

<pre>sudo ip addr

1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether fa:16:3e:d4:31:b5 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.10/24 brd 10.99.99.255 scope global dynamic eth0
       valid_lft 60138sec preferred_lft 60138sec
    inet6 fe80::f816:3eff:fed4:31b5/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: &lt;BROADCAST,MULTICAST&gt; mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether fa:16:3e:f2:65:4a brd ff:ff:ff:ff:ff:ff</pre>

Buat berkas Konfigurasi eth1

<pre>sudo cp -v /etc/sysconfig/network-scripts/ifcfg-eth0 /etc/sysconfig/network-scripts/ifcfg-eth1</pre>

Sesuaikan _Device_ dan _HWADDR_

<pre>sudo vi /etc/sysconfig/network-scripts/ifcfg-eth1

BOOTPROTO=dhcp
DEVICE=eth1
HWADDR=fa:16:3e:f2:65:4a
ONBOOT=yes
TYPE=Ethernet
USERCTL=no
DEFROUTE=no</pre>

Bring up eth1

<pre>sudo ifup eth1</pre>

Verifikasi

<pre>sudo ip addr
1: lo: &lt;LOOPBACK,UP,LOWER_UP&gt; mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether fa:16:3e:d4:31:b5 brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.10/24 brd 10.99.99.255 scope global dynamic eth0
       valid_lft 60138sec preferred_lft 60138sec
    inet6 fe80::f816:3eff:fed4:31b5/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether fa:16:3e:f2:65:4a brd ff:ff:ff:ff:ff:ff
    inet 10.11.11.10/24 brd 10.199.199.255 scope global dynamic eth1
       valid_lft 85976sec preferred_lft 85976sec
    inet6 fe80::f816:3eff:fef2:654a/64 scope link 
       valid_lft forever preferred_lft forever</pre>

Cara Ke-2:  
Dengan menambahkan **Configuration > Customisation Script** berikut pada saat membuat Instance baru dengan _Multiple Interface_:

<pre>#!/bin/bash
sudo -i
echo "dhclient" >> /etc/rc.local
chmod +x /etc/rc.local
/etc/rc.local
</pre>

**Ubuntu 16.04**

<pre>vim K99multinic.sh</pre>

<pre>#!/bin/bash
hname=$(hostname)
cat /etc/hosts | grep $hname >> /dev/null
if [ $? -ne 0 ];then
 sudo bash -c "echo '127.0.0.1 $hname' >> /etc/hosts"
fi
netfile=$(find /etc/network/interfaces.d -name "*.cfg")
for interface in $(ls -1 /sys/class/net | grep ens) ;do
   cat $netfile | grep $interface >> /dev/null
   if [ $? -ne 0 ];then
     sudo bash -c "echo 'auto $interface' >> ${netfile}"
     sudo bash -c "echo 'iface $interface inet dhcp' >> ${netfile}"
     sudo ifup $interface
  fi
done</pre>

Ubah permission dan Eksekusi skrip

<pre>chmod +x K99multinic.sh
./K99multinic.sh</pre>

Ubah kepemilikan lalu simpan ke /etc/rc0.d/ agar skrip dieksekusi saat reboot

<pre>sudo chown root:root K99multinic.sh
sudo mv K99multinic.sh /etc/rc0.d/ </pre>

Ujicoba reboot Instance

<pre>sudo reboot</pre>

Sekian dan Selamat Liburan. 🙂

* * *

**Refrensi: ** <a href="https://support.metacloud.com/hc/en-us/articles/115002332667-Configuring-a-VM-to-work-with-multiple-network-interfaces" target="_blank" rel="noopener">https://support.metacloud.com/hc/en-us/articles/115002332667-Configuring-a-VM-to-work-with-multiple-network-interfaces</a>  
<a href="https://leadwithoutatitle.wordpress.com/2017/07/06/how-to-create-a-multi-nic-ubuntu-cloud-image/" rel="noopener" target="_blank">https://leadwithoutatitle.wordpress.com/2017/07/06/how-to-create-a-multi-nic-ubuntu-cloud-image/</a>