---
id: 86
title: Membuat Container di Proxmox VE
date: 2015-02-23T22:30:27+07:00
author: ary
layout: post
guid: http://aryulianto.com/?p=86
permalink: /membuat-container-di-proxmox-ve/
categories:
  - Linux
tags:
  - Linux
  - OpenSource
  - Proxmox
  - Virtualisasi
---
1. Pastikan kita sudah punya _template_ container OpenVZ, jika belum punya bisa dicari dan unduh di <span style="color: #800000;"><a style="color: #800000;" title="http://openvz.org/Download/template/precreated" href="http://openvz.org/Download/template/precreated" target="_blank">http://openvz.org/Download/template/precreated</a></span>

2. Jika sudah unduh atau memiliki _template_ OpenVZ simpan filenya di folder

<pre>/var/lib/vz/template/cache</pre>

3. Sekarang masuk ke webpanel proxmox, masukan <span style="color: #800000;"><a style="color: #800000;" title="https://ip_proxmox:8006" href="https://ip_proxmox:8006" target="_blank">https://ip_proxmox:8006</a> </span>pada browser kesayangan anda. port 8006 merupakan port standar webpanel proxmox.

4. Selanjutnya masuk dengan memasukan <span id="result_box" class="short_text" lang="id"><span class="hps">nama pengguna</span></span> dan kata sandi anda.

5. Setelah masuk ke webpanel proxmox, klik _“Create CT”_ di pojok kanan atas untuk membuat _container_ _baru,_ isikan hostname dan kata sandi sesuai dengan yang diinginkan, jika sudah klik Next.

[<img class="alignnone wp-image-165 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_1-1024x576.png" alt="" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_1-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_1-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_1-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_1.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_1.png)

6. Selanjutnya pilih _template_ yang tadi sudah disalin di langkah ke-2 klik Next.

[<img class="alignnone size-large wp-image-166" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_2-1024x576.png" alt="OVZ_2" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_2-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_2-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_2-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_2.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_2.png)

7. Konfigurasi jumlah _memory (MB), swap (MB), disk size (GB)_ dan _CPU_ sesuai kebutuhan. klik Next.

8. Selanjutnya konfigurasi _Network_ sesuai kebutuhan. klik Next

[<img class="alignnone size-large wp-image-167" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_3-1024x576.png" alt="OVZ_3" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_3-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_3-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_3-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_3.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_3.png)

9. Lalu konfigurasikan _DNS_ (_Domain Name Service)_ klik Next.

[<img class="alignnone size-large wp-image-168" src="http://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_4-1024x576.png" alt="OVZ_4" width="730" height="411" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_4-1024x576.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_4-300x169.png 300w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_4-1200x675.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_4.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2015/02/OVZ_4.png)

10. Pastikan kembali semua konfigurasi di tab _Confirm_, jika sudah benar dan yakin klik Finish. Jika muncul keluaran **TASK OK** maka proses berhasil.

11. Jika sudah berhasil klik kanan _container_ yang baru kita buat, lalu klik _Start  
_ dan _container_ siap untuk digunakan.

* * *

**Refrensi**: <span style="color: #999999;">https://sdmokonote.wordpress.com/2015/02/12/memasang-container-openvz-di-proxmox/</span>