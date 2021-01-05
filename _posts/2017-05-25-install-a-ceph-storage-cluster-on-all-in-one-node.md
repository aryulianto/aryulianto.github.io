---
id: 651
title: Install a Ceph Storage Cluster on All in One Node
date: 2017-05-25T17:54:12+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=651
permalink: /install-a-ceph-storage-cluster-on-all-in-one-node/
categories:
  - Linux
tags:
  - ceph
  - cluster
  - openSUSE
  - storage
---
> Mari langsung saja, cara membuat Ceph Storage Cluster di openSUSE Leap dalam satu Node.

[<img class="aligncenter size-full wp-image-688" src="http://blog.aryulianto.com/wp-content/uploads/2017/05/ceph_opensuse.png" alt="" width="634" height="303" srcset="https://blog.aryulianto.com/wp-content/uploads/2017/05/ceph_opensuse.png 634w, https://blog.aryulianto.com/wp-content/uploads/2017/05/ceph_opensuse-300x143.png 300w" sizes="(max-width: 634px) 100vw, 634px" />](http://blog.aryulianto.com/wp-content/uploads/2017/05/ceph_opensuse.png)

Persiapan

  * Hostname: ceph-aio
  * OS: openSUSE 42.2
  * 3 Disk tambahan untuk OSD
  * Akses Internet

Pasang paket _ceph_ dan _ceph-deploy_

<pre>zypper -y install ceph
zypper -y install ceph-deploy</pre>

Buat user baru untuk _ceph-deploy_

<pre>useradd -m -s /bin/bash ary
passwd ary</pre>

Konfigurasi agar user _ceph-deploy _tadi memiliki privileges root

<pre>echo "ary ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ary
chmod 0440 /etc/sudoers.d/ary</pre>

Masuk sebagai pengguna baru yang tadi telah dibuat, lalu generate kunci publik untuk ssh tanpa sandi

<pre>sudo su - ary
ssh-keygen
ssh-copy-id ary@ceph-aio</pre>

Buat direktori baru untuk _ceph-deploy_

<pre>cd ~
mkdir my-cluster
cd my-cluster</pre>

Buat konfigurasi cluster di direktori ini

<pre>ceph-deploy new ceph-aio
ls -lh</pre>

Setelah _ceph-deploy new_ maka akan terbuat file konfigurasi baru di direktori tadi, set replikasi menjadi = 2  dan type = 0 karena kita menggunakan AIO node

<pre>echo "osd pool default size = 2" &gt;&gt; ceph.conf
echo "osd crush chooseleaf type = 0" &gt;&gt; ceph.conf
echo "public network = 10.7.7.0/24" &gt;&gt; ceph.conf</pre>

Pasang Ceph

<pre>ceph-deploy install ceph-aio
sudo chown -R ceph:ceph /var/lib/ceph/mon</pre>

Buat monitor awal

<pre>ceph-deploy mon create ceph-aio
ceph-deploy mon create-initial</pre>

Generate kunci untuk autentikasi

<pre>ceph-deploy gatherkeys ceph-aio</pre>

Setelah selesai, harusnya direktori  terdapat _keyrings_ berikut:

<pre>{cluster-name}.client.admin.keyring
{cluster-name}.bootstrap-osd.keyring
{cluster-name}.bootstrap-mds.keyring
{cluster-name}.bootstrap-rgw.keyring</pre>

Buat partisi storage

<pre>sudo /dev/vdX

vdX type =  83

sudo partprobe
sudo fdisk -l</pre>

Format dan mounting disk

<pre>sudo mkfs.xfs /dev/vdb1
sudo mkdir -p /var/local/osd-vdb1
sudo mkfs.xfs /dev/vdc1
sudo mkdir -p /var/local/osd-vdc1
sudo mkfs.xfs /dev/vdd1
sudo mkdir -p /var/local/osd-vdd1
exit
echo "/dev/vdb1 /var/local/osd-vdb1 xfs defaults 0 0" &gt;&gt;/etc/fstab
echo "/dev/vdc1 /var/local/osd-vdc1 xfs defaults 0 0" &gt;&gt;/etc/fstab
echo "/dev/vdd1 /var/local/osd-vdd1 xfs defaults 0 0" &gt;&gt;/etc/fstab
mount -a
mount | grep vd
df -h</pre>

Ubah permission di direktori yang sudah dibuat tadi

<pre>sudo su - ary
cd ~/my-cluster
sudo chown ceph:ceph /var/local/osd*</pre>

Jalankan prepare dan activate untuk disk OSD

<pre>ceph-deploy osd prepare ceph-aio:/var/local/osd-vdb1 ceph-aio:/var/local/osd-vdc1 ceph-aio:/var/local/osd-vdd1
ceph-deploy osd activate ceph-aio:/var/local/osd-vdb1 ceph-aio:/var/local/osd-vdc1 ceph-aio:/var/local/osd-vdd1</pre>

Salin konfigurasi dan key admin

<pre>ceph-deploy admin ceph-aio</pre>

Set permission

<pre>sudo chmod +r /etc/ceph/ceph.client.admin.keyring</pre>

Cek cluster dan osd status

<pre>ceph -s
ceph osd tree</pre>

Tes membuat pool dan operasi object data

<pre>ceph osd pool create pool-01 128
echo coba &gt; coba.txt
echo test &gt; test.txt
echo okay &gt; okay.txt
rados put object-01 coba.txt --pool=pool-01
rados put object-02 test.txt --pool=pool-01
rados put object-03 okay.txt --pool=pool-01
rados ls --pool=pool-01</pre>

Verifikasi

<pre>sudo find /var/local/osd* -name *object-01*
sudo find /var/local/osd* -name *object-02*
sudo find /var/local/osd* -name *object-03*</pre><article id="post-591" class="post post-591 type-post status-publish format-standard hentry category-linux tag-linux tag-proxmox tag-virtualisasi"> 

<div class="entry-content">
  <p>
    <strong>~Sekian dan Semoga bermanfaat. </strong>
  </p>
  
  <h6>
    <del>ps: panduan ini sangat tidak disarankan untuk keperluan produksi</del> 😛
  </h6>
  
  <hr />
  
  <p>
    <strong>Refrensi:  </strong><a href="http://docs.ceph.com">http://docs.ceph.com<br /> </a><a href="http://docs.ceph.com">http://docs.ceph.com/docs/master/start/quick-ceph-deploy/<br /> </a><a href="http://palmerville.github.io/2016/04/30/single-node-ceph-install.html">http://palmerville.github.io/2016/04/30/single-node-ceph-install.html</a>
  </p>
</div></article>