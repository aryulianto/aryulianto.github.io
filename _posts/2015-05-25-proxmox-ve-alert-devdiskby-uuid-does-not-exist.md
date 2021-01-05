---
id: 310
title: 'Proxmox VE : ALERT /dev/disk/by-uuid does not exist'
date: 2015-05-25T11:53:09+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=310
permalink: /proxmox-ve-alert-devdiskby-uuid-does-not-exist/
categories:
  - Linux
tags:
  - Linux
  - OpenSource
  - Proxmox
  - Virtualisasi
---
<pre>ALERT! /dev/disk/by-uuid/32629f4d-4bd3-41df-b3d6-deac7c677311 does not exist.
Dropping to a shell!</pre>

[<img class="aligncenter size-large wp-image-346" src="http://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_1-1024x575.jpg" alt="Kode_Kesalahan_1" width="730" height="410" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_1-1024x575.jpg 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_1-300x169.jpg 300w, https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_1-1200x674.jpg 1200w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_1.jpg)  
Okeh, setelah tanya sana sini di google akhirnya nemu juga orang yg ngalamin hal yang sama.

1. Saya langsung segera membuat bootable usb dengan isi <a title="GParted" href="http://gparted.org/" target="_blank">GParted</a> filenya yang hanya 227 MB tak butuh waktu lama untuk saya <a title="Unduh" href="http://softlayer-sng.dl.sourceforge.net/project/gparted/gparted-live-stable/0.22.0-1/gparted-live-0.22.0-1-amd64.isohttp://" target="_blank">unduh</a> dan buatkan bootablenya (ingat pilih yang x64 karena saya menghabiskan waktu 1 jam karena salah unduh)

2. Kaitkan /dev/sda1 (partisi / boot dari instalasi Proxmox) ke /mnt

<pre><strong>mount /dev/sda1 /mnt</strong></pre>

3. Cek file device.map dan grub.cfg

<pre><strong>cat /mnt/grub/device.map</strong>
<strong>cat /mnt/grub/grub.cfg</strong></pre>

[<img class="aligncenter size-large wp-image-326" src="http://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_2-e1432489632936-1024x256.jpg" alt="Kode_Kesalahan_2" width="730" height="183" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_2-e1432489632936-1024x256.jpg 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_2-e1432489632936-300x75.jpg 300w, https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_2-e1432489632936-1200x300.jpg 1200w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_2-e1432489632936.jpg)

4. Selanjutnya Mari kita memeriksa UUID dari partisi root. Pertama mengaktifkan kelompok volume PvE:

<pre><strong>vgchange -ay pve</strong>
 3 logical volume(s) in volume group "pve" now active</pre>

5. Dimana titik LV root ?

<pre><strong>ls -l /dev/mapper | grep pve-root</strong>
lrwxrwxrwx 1 root root 7 Feb  2 09:06 pve-root -&gt; ../dm-1</pre>

6. Lihat UUID dari / dev / dm-1:

<pre><strong>ls -l /dev/disk/by-uuid | grep dm-1</strong>
lrwxrwxrwx 1 root root 10 Feb  2 09:06 32629f4d-4bd3-41df-b3d6-deac7c67731</pre>

7. Lepaskan kaitan dan kaitkan lagi seperti kode dibawah

<pre>umount /mnt

<strong>mount /dev/mapper/pve-root /mnt
mount /dev/sda1 /mnt/boot
mount --bind /dev /mnt/dev
mount --bind /proc /mnt/proc
mount --bind /sys /mnt/sys</strong></pre>

8. Change root untuk masuk kedalam system Proxmox

<pre><strong> chroot /mnt</strong></pre>

9. lalu Update grub

<pre><strong>update-grub </strong></pre>

10. Jika menemukan kode kesalahan

<pre>/usr/sbin/grub-probe: error: Couldn't find PV pv1. Check your device.map.</pre>

Ubah file /mnt/grub/device.map menjadi (hd01) /dev/sda1

<pre><strong>nano /mnt/grub/device.map</strong>
(hd01) /dev/sda1</pre>

11. Vgdisplay tidak menunjukkan apa-apa. sebelum mengaktifkan VG PvE lagi:

<pre><strong>vgchange -ay pve
</strong>  3 logical volume(s) in volume group "pve" now active

<strong>pvdisplay</strong>
--- Physical volume ---
PV Name        /dev/sda2
VG Name        pve
[...]
PV UUID        1YMsls-ycW4-aqWL-MEsC-EOFM-VVyI-4UrgFY
[...]</pre>

12. Jalankan update-grub lagi

<pre><strong>update-grub</strong></pre>

13. Saat update-grub system tidak memunculkan pesan kesalahan. Mari kita periksa kembali isi dari device.map dan grub.cfg :

<pre><strong>cat /boot/grub/device.map</strong>
(hd0) /dev/sda1
<strong>cat /boot/grub/grub.cfg </strong></pre>

14. Jika sudah benar, mari kita reboot dan lihat hasilnya. yeay mesin kembali berjalan tanpa kode kesalahan dan sedang memeriksa media penyimpanan tunggu sampai selesai dan mesin sudah kembali normal.  
[<img class="aligncenter size-large wp-image-339" src="http://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_3-1024x575.jpg" alt="Kode_Kesalahan_3" width="730" height="410" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_3-1024x575.jpg 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_3-300x169.jpg 300w, https://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_3-1200x674.jpg 1200w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/05/Kode_Kesalahan_3.jpg)

* * *

Refrensi: <a href="http://www.claudiokuenzler.com/blog/442/grub2-boot-issue-error-alert-dev-disk-by-uuid-does-not-exist#.VWQI22F1i-k" target="_blank">http://www.claudiokuenzler.com/blog/442/grub2-boot-issue-error-alert-dev-disk-by-uuid-does-not-exist#.VWQI22F1i-k</a>