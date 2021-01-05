---
id: 217
title: 'Vyatta &#8211; Dasar Konfigurasi setelah instalasi'
date: 2015-03-15T18:56:18+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=217
permalink: /vyatta-dasar-konfigurasi-setelah-instalasi/
categories:
  - Linux
tags:
  - Linux
  - OpenSource
  - Router
  - Vyatta
---
<span id="result_box" class="" lang="id"><span class="hps">Setelah memulai</span> <span class="hps">mesin</span> <a href="http://www.brocade.com/launch/vyatta/" target="_blank"><span class="hps">Vyatta</span></a>. <span class="hps">kalian harus login terlebih dahulu, isikan nama pengguna dengan <strong><em>vyatta</em></strong> dan <strong><em>vyatta </em></strong>juga untuk kata sandinya. berikut ini adalah skenario topologi yang akan kita buat<br /> </span></span>

&nbsp;

[<img class="aligncenter wp-image-276 size-full" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Diagram1.png" alt="Diagram1" width="431" height="212" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Diagram1.png 431w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Diagram1-300x148.png 300w" sizes="(max-width: 431px) 100vw, 431px" />](http://blog.aryulianto.com/wp-content/uploads/2015/03/Diagram1.png)

&nbsp;

<span class="" lang="id"><span class="hps"> Pertama-tama ketikan perintah dibawah ini untuk memasang</span></span>

<pre class="font-size:13 lang:default decode:true">$ install image</pre>

<div id="wp-spoiler-2" class="wp-spoiler wpui-hashable wpui-light wpui-styles" data-style="wpui-light">
  <h3 class="wp-spoiler-title wpui-hashable fade-true slide-true open-false">
    FYI Vyatta Command Mode:
  </h3>
  
  <div class="wpui-hidden wp-spoiler-content">
    1) Operational mode ($)<br /> Mode operasional menyediakan akses ke perintah yang berkaitan dengan menunjukkan, jelas, mengaktifkan atau menonaktifkan debugging, serta perintah untuk mengkonfigurasi pengaturan terminal, memuat dan menyimpan konfigurasi, dan me-restart sistem.</p> 
    
    <p>
      2) Configuration mode (#)<br /> Mode konfigurasi menyediakan akses ke perintah untuk membuat, mengubah, menghapus, melakukan dan menunjukkan informasi konfigurasi, serta perintah untuk menavigasi melalui hirarki konfigurasi.</div> </div><!-- end div.wp-spoiler -->
      
      <br /> 1. Membuat user baru dengan hak admin, hal ini wajib dilakuin karena semua default login vyatta sama, caranya masuk mode configurasi lalu :
    </p>
    
    <pre class="font-size:13 lang:default decode:true"># set system login user "namauser" authentication plaintext-password "password"
# set system login user "namauseryangtadi" level admin
# commit</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_1.png"><img class="alignnone size-full wp-image-233" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_1.png" alt="Vy_1" width="720" height="106" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_1.png 720w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_1-300x44.png 300w" sizes="(max-width: 720px) 100vw, 720px" /></a><br /> Verifikasi user baru yang tadi telah dibuat
    </p>
    
    <pre class="theme:coda-special-board font-size:13 lang:default decode:true "># show system login user "usernameyangtadi"</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_2.png"><img class="alignnone size-full wp-image-235" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_2.png" alt="Vy_2" width="720" height="152" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_2.png 720w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_2-300x63.png 300w" sizes="(max-width: 720px) 100vw, 720px" /></a><br /> Reboot lalu login dengan user yang baru, selanjutnya hapus user default vyatta nya
    </p>
    
    <pre class="font-size:13 lang:default decode:true"># delete system login user vyatta
# commit
# save</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_3.png"><img class="alignnone size-full wp-image-237" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_3.png" alt="Vy_3" width="720" height="167" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_3.png 720w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_3-300x70.png 300w" sizes="(max-width: 720px) 100vw, 720px" /></a><br /> 2. Setting host name, nama host name default untuk perangkat Vyatta adalah vyatta. Gunakan perintah berikut untuk mengubah nama host name.
    </p>
    
    <pre># set system host-name "namahostname"
# commit
# save</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_4.png"><img class="alignnone size-full wp-image-257" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_4.png" alt="Vy_4" width="720" height="194" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_4.png 720w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_4-300x81.png 300w" sizes="(max-width: 720px) 100vw, 720px" /></a><br /> <strong>PS</strong>: <em>nama hostname akan terganti saat anda reboot dan login kembali</em>
    </p>
    
    <p>
      3. <span id="result_box" class="short_text" lang="id"><span class="hps">Untuk melihat</span> <span class="hps">antarmuka</span> <span class="hps">pada</span> <span class="hps">Vyatta</span> gunakan perintah<br /> </span>
    </p>
    
    <pre>$ show interfaces</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_8.png"><img class="alignnone size-full wp-image-262" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_8.png" alt="Vy_8" width="720" height="182" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_8.png 720w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_8-300x76.png 300w" sizes="(max-width: 720px) 100vw, 720px" /></a><br /> 4. Konfigurasi ip DHCP pada eth0
    </p>
    
    <pre># set interfaces ethernet eth0 address dhcp
# commit
# save</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_6.png"><img class="alignnone size-full wp-image-263" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_6.png" alt="Vy_6" width="720" height="213" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_6.png 720w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_6-300x89.png 300w" sizes="(max-width: 720px) 100vw, 720px" /></a><br /> 5. Konfigurasi ip STATIC pada eth1
    </p>
    
    <pre># set interfaces ethernet eth1 address "ipaddress/prefix"
# commit
# save</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_7.png"><img class="alignnone size-full wp-image-267" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_7.png" alt="Vy_7" width="720" height="170" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_7.png 720w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_7-300x71.png 300w" sizes="(max-width: 720px) 100vw, 720px" /></a><br /> 6. Mengaktifkan akses SSH<br /> Sebelum mengizinkan akses SSH, kita harus mengaktifkan layanan SSH pada sistem vyatta. Kita juga bisa mengubah port SSH (default adalah 22) untuk alasan tertentu 😀
    </p>
    
    <pre># set service ssh
# set service ssh port "noport"
# commit
# save</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_9.png"><img class="alignnone size-full wp-image-269" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_9.png" alt="Vy_9" width="720" height="247" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_9.png 720w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_9-300x103.png 300w" sizes="(max-width: 720px) 100vw, 720px" /></a>
    </p>
    
    <p>
      7. Uji Coba dari klien
    </p>
    
    <p>
      Linux : buka terminal (bukan terminal bis lho ya :p) lalu ketikan perintah
    </p>
    
    <pre>ssh -l ary 10.69.69.1 -p 6969
ssh -l "namauser" "iprouter" -p "port"</pre>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/RHEL_2.png"><img class="alignnone wp-image-279 size-large" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/RHEL_2-1024x768.png" alt="RHEL" width="730" height="548" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/RHEL_2.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2015/03/RHEL_2-300x225.png 300w" sizes="(max-width: 730px) 100vw, 730px" /></a>
    </p>
    
    <p>
      Windows : Buka <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html">Putty</a>, yang belum punya bisa download <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html">disini </a>masukan IP Router dan sesuaikan Portnya lalu klik Open
    </p>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_11.png"><img class="alignnone size-full wp-image-280" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_11.png" alt="Vy_11" width="455" height="435" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_11.png 455w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_11-300x287.png 300w" sizes="(max-width: 455px) 100vw, 455px" /></a>
    </p>
    
    <p>
      lalu masukan password dan kalian sudah berhasil ssh ke vyatta
    </p>
    
    <p>
      <a href="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_12.png"><img class="alignnone size-full wp-image-281" src="http://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_12.png" alt="Vy_12" width="672" height="424" srcset="https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_12.png 672w, https://blog.aryulianto.com/wp-content/uploads/2015/03/Vy_12-300x189.png 300w" sizes="(max-width: 672px) 100vw, 672px" /></a>
    </p>
    
    <hr />
    
    <p>
      <strong>Refrensi:</strong> Vyatta/VyOS Fundamental by. <a href="https://twitter.com/sdmoko" target="_blank">Syah Dwi Phihatmoko </a>at <a title="http://lokan.btech.co.id" href="http://lokan.btech.co.id" target="_blank">http://lokan.btech.co.id</a> | <span style="color: #999999;"><a style="color: #999999;" title="https://rbgeek.wordpress.com/2013/05/06/vyatta-basic-configuration-after-installation/" href="https://rbgeek.wordpress.com/2013/05/06/vyatta-basic-configuration-after-installation/" target="_blank">https://rbgeek.wordpress.com/2013/05/06/vyatta-basic-configuration-after-installation/</a></span>
    </p>