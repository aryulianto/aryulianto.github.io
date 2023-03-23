---
id: 1141
title: 'Mengamankan SSH dengan Google Authenticator &#8211; openSUSE Leap 15'
date: 2019-01-12T21:15:00+07:00
author: ary
layout: post
guid: https://blog.aryulianto.com/?p=1141
permalink: /mengamankan-ssh-dengan-google-authenticator-opensuse-leap-15/
categories:
  - Linux
tags:
  - Leap
  - Linux
  - MFA
  - openSUSE
  - SSH
---
Bagi saya, akses SSH ke _Jump server, Jump host_ atau _Jumpbox_ menggunakan _Public Key _adalah mandatori, kali ini saya akan menambahkan _Multi-factor Authentication_ (MFA) untuk akses SSH ke _Jump server_ saya. Berikut langkah-langkahnya:

1. Pasang Repositori

<pre>zypper addrepo https://download.opensuse.org/repositories/security/openSUSE_Leap_15.0/security.repo
zypper refresh</pre>

2. Pasang paket Google Authenticator

<pre>zypper install -y pam-google-authenticator</pre>

3. Jalankan Google Authenticator

<pre>google-authenticator</pre>

[<img class="alignnone size-large wp-image-1162" src="http://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-1024x463.png" alt="" width="730" height="330" srcset="https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-1024x463.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-300x136.png 300w, https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-768x347.png 768w, https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-1200x542.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth.png 1233w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth.png)

Scan barcode atau masukan **Your new secret key** ke aplikasi <a href="https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=en" target="_blank" rel="noopener">Google Authenticator</a>

[<img src="http://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-apps-1024x491.png" alt="" width="730" height="350" class="alignnone size-large wp-image-1182" srcset="https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-apps-1024x491.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-apps-300x144.png 300w, https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-apps-768x369.png 768w, https://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-apps-1200x576.png 1200w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2019/01/google-auth-apps.png)

4. Sunting berkas Konfigurasi SSH

<pre>vim /etc/ssh/sshd_config

ChallengeResponseAuthentication yes 
AuthenticationMethods publickey,keyboard-interactive
</pre>

5. _Disable_ common-auth dan tambahkan auth google

<pre>vim /etc/pam.d/sshd

#@include common-auth 
auth required pam_google_authenticator.so
</pre>

6. Muat ulang layanan SSH

<pre>systemctl restart sshd
systemctl status sshd
</pre>

7. Ujicoba SSH ke Jump server

<pre>ssh -l ary gateway.aryulianto.com -p2222
Authenticated with partial success.
Verification code: 
</pre>

Sekarang kalian akan dimintai kode Google Authenticator setiap kali Anda mencoba masuk melalui SSH.

**Sekian dan Terima kasih**
