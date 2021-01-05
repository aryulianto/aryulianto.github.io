---
id: 48
title: Install Proxmox VE di VM VirtualBox
date: 2015-02-22T20:44:04+07:00
author: ary
layout: post
guid: http://aryulianto.com/?p=48
permalink: /dummy-post/
categories:
  - Linux
tags:
  - Linux
  - OpenSource
  - Proxmox
  - Virtualisasi
---
Proxmox VE merupakan sebuah distro Linux turunan dari Debian (64 bit)  
berbasis _CLI (Command line Interface)_. Proses installasi dan administrasi Proxmox VE ini cukup keren karena berbasis Web. Proxmox VE ini sendiri juga dimanfaatkan untuk membuat cloud computing.

Berikut ini saya akan membagikan sedikit cara untuk menginstall Proxmox di Virtual Box.

1. Buka virtual mesin VM VirtualBox yang telah terinstall, karena kita akan menginstall virtual mesin yang baru maka pilih _New_ pada menu.

2. Pada Kotak dialog _Create Virtual Machine_, isi nama sesuai keinginan anda,Untuk _Type_ dan _Version_. Kemudian klik Next untuk melanjutkan ke proses selanjutnya

[<img class="alignnone wp-image-151 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_01-1024x576.png" alt="VB_01" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_01-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_01-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_01-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_01.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_01.png)

3. Kemudian atur kapasitas memori yang dibutuhkan. Sebenarnya sudah direkomendsikan oleh si Virtual Mesin, namun disarankan untuk mengisi 1024 MB keatas. Setelah itu klik Next.

[<img class="alignnone wp-image-152 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_02-1024x576.png" alt="VB_02" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_02-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_02-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_02-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_02.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_02.png)

4. Pada Kotak dialog _Hard Drive_ pilih Create a virtual Hard drive now. Kemudian klik _Create._

[<img class="alignnone wp-image-153 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_03-1024x576.png" alt="VB_03" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_03-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_03-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_03-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_03.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_03.png)

5. Pada Kotak Dialog _Hard drive_ file type pilih _VDI (VirtualBox Disk Image)_ Kemudian klik Next.

6. Pada Kotak dialog _Storage_ on physical hard drive pilih _Dynamically allocated_. Kemudian klik Next.

7. Pada Kotak Dialog _File location and Size_ tentukan lokasi penyimpanan data Jika partisi telah siap, dan ukuran memori penyimpanan _hard disk_. Kemudain klik _Create._

[<img class="alignnone wp-image-154 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_04-1024x576.png" alt="VB_04" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_04-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_04-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_04-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_04.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_04.png)  
8. Kemudian masukan file ISO Proxmox kedalam virtual box, lalu klik _Start_.

[<img class="alignnone wp-image-155 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_05-1024x576.png" alt="VB_05" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_05-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_05-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_05-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/VB_05.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/VB_05.png)

9. Yap proses intallasi pun dimulai, di layar _boot loader_ tekan saja enter untuk melanjutkan.

[<img class="alignnone wp-image-157 size-full" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/1.jpeg" alt="1" width="640" height="480" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/1.jpeg 640w, https://blog.aryulianto.com/wp-content/uploads/2015/02/1-300x225.jpeg 300w" sizes="(max-width: 640px) 100vw, 640px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/1.jpeg)

10. Kemudian pilih _Agree_ di layar lisensi.

[<img class="alignnone wp-image-158 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/2-1024x768.jpeg" alt="2" width="730" height="548" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/2.jpeg 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/2-300x225.jpeg 300w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/2.jpeg)

11. Pada tahap ini pastikan kembali target _harddisk_ yang akan digunakan. Klik Next untuk melanjutkan instalasi Proxmox VE.

[<img class="alignnone wp-image-159 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/3-1024x768.jpeg" alt="3" width="730" height="548" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/3.jpeg 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/3-300x225.jpeg 300w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/3.jpeg)

12. Di layar pengaruran _Location and Time Zone_, pilih Indonesia dan zona waktu yang sesuai. Klik Next untuk melanjutkan instalasi Proxmox VE.

[<img class="alignnone wp-image-160 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/4-1024x768.jpeg" alt="4" width="730" height="548" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/4.jpeg 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/4-300x225.jpeg 300w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/4.jpeg)

13. Selanjutnya tentukan kata sandi dan surel untuk pengguna root yang nantinya akan digunakan untuk mengatur server virtualisasi Proxmox VE ini.

[<img class="alignnone wp-image-161 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/5-1024x768.jpeg" alt="5" width="730" height="548" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/5.jpeg 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/5-300x225.jpeg 300w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/5.jpeg)

14. Kemudian lakukan pengaturan nama _host, alamat IP, default gateway, dan DNS_ untuk server Proxmox VE.

15. Tunggu hingga proses Instalasi Proxmox VE selesai. Di akhir instalasi instalasi klik _Reboot_ untuk masuk ke sistem Proxmox VE yang baru saja diinstall.

* * *

**Refrensi: **<span style="color: #999999;">http://erela19.blogspot.com | http://ilmukomputer.com </span>