---
id: 179
title: Install Telegram Messenger di CLI
date: 2015-03-01T18:10:12+07:00
author: ary
layout: post
guid: http://aryulianto.com/?p=179
permalink: /install-telegram-messenger-di-cli/
categories:
  - Linux
---
1. Pertama clone dulu GitHub Repositorynya

<pre>git clone --recursive https://github.com/vysheng/tg.git && cd tg</pre>

2. Install paket-paket pendukungnya, karena saya menggunakan Debian maka perintahnya

<pre>sudo apt-get install libreadline-dev libconfig-dev libssl-dev lua5.2 liblua5.2-dev libevent-dev make</pre>

3. Jika mendapati kode kesalahan seperti ini

<pre><span style="color: #993300;">Submodule path 'tgl/tl-parser': checked out 'ec8a8ed7a4f22428b83e21a9d3b5815f7a6f3bd9'</span></pre>

maka lakukan

<pre>git submodule update --init --recursive</pre>

Selanjutnya jalankan perintah

<pre>./configure</pre>

<pre>make</pre>

4. Tunggu hinggra prosesnya selesai, lalu jalankan Telegram Cli dengan perintah

<pre>bin/telegram-cli -k tg-server.pub</pre>

5. Saat pertama menginstall kalian akan diperintahkan memasukkan nomor handphone untuk pendaftaran, selanjutnya tunggu dan masukkan kode konfirmasinya. Selesai

<div id="wp-spoiler-1" class="wp-spoiler wpui-hashable wpui-light wpui-styles" data-style="wpui-light">
  <h3 class="wp-spoiler-title wpui-hashable fade-true slide-true open-false">
    Beberapa contoh baris perintah Telegram di CLI
  </h3>
  
  <div class="wpui-hidden wp-spoiler-content">
    </p> 
    
    <h4>
      <a id="user-content-messaging" class="anchor" href="https://github.com/vysheng/tg/blob/master/README.md#messaging"></a>Messaging
    </h4>
    
    <ul class="task-list">
      <li>
        <strong>msg</strong> <peer> Text &#8211; sends message to this peer
      </li>
      <li>
        <strong>fwd</strong> <user> <msg-seqno> &#8211; forward message to user. You can see message numbers starting client with -N
      </li>
      <li>
        <strong>chat_with_peer</strong> <peer> starts one on one chat session with this peer. /exit or /quit to end this mode.
      </li>
      <li>
        <strong>add_contact</strong> <phone-number> <first-name> <last-name> &#8211; tries to add contact to contact-list by phone
      </li>
      <li>
        <strong>rename_contact</strong> <user> <first-name> <last-name> &#8211; tries to rename contact. If you have another device it will be a fight
      </li>
      <li>
        <strong>mark_read</strong> <peer> &#8211; mark read all received messages with peer
      </li>
      <li>
        <strong>delete_msg</strong> <msg-seqno> &#8211; deletes message (not completly, though)
      </li>
      <li>
        <strong>restore_msg</strong> <msg-seqno> &#8211; restores delete message. Impossible for secret chats. Only possible short time (one hour, I think) after deletion
      </li>
    </ul>
    
    <h4>
      <a id="user-content-multimedia" class="anchor" href="https://github.com/vysheng/tg/blob/master/README.md#multimedia"></a>Multimedia
    </h4>
    
    <ul class="task-list">
      <li>
        <strong>send_photo</strong> <peer> <photo-file-name> &#8211; sends photo to peer
      </li>
      <li>
        <strong>send_video</strong> <peer> <video-file-name> &#8211; sends video to peer
      </li>
      <li>
        <strong>send_text</strong> <peer> <text-file-name> &#8211; sends text file as plain messages
      </li>
      <li>
        <strong>load_photo</strong>/load_video/load_video_thumb/load_audio/load_document/load_document_thumb <msg-seqno> &#8211; loads photo/video/audio/document to download dir
      </li>
      <li>
        <strong>view_photo</strong>/view_video/view_video_thumb/view_audio/view_document/view_document_thumb <msg-seqno> &#8211; loads photo/video to download dir and starts system default viewer
      </li>
      <li>
        <strong>fwd_media</strong> <msg-seqno> send media in your message. Use this to prevent sharing info about author of media (though, it is possible to determine user_id from media itself, it is not possible get access_hash of this user)
      </li>
      <li>
        <strong>set_profile_photo</strong> <photo-file-name> &#8211; sets userpic. Photo should be square, or server will cut biggest central square part
      </li>
    </ul>
    
    <h4>
      <a id="user-content-group-chat-options" class="anchor" href="https://github.com/vysheng/tg/blob/master/README.md#group-chat-options"></a>Group chat options
    </h4>
    
    <ul class="task-list">
      <li>
        <strong>chat_info</strong> <chat> &#8211; prints info about chat
      </li>
      <li>
        <strong>chat_add_user</strong> <chat> <user> &#8211; add user to chat
      </li>
      <li>
        <strong>chat_del_user</strong> <chat> <user> &#8211; remove user from chat
      </li>
      <li>
        <strong>rename_chat</strong> <chat> <new-name>
      </li>
      <li>
        <strong>create_group_chat</strong> <chat topic> <user1> <user2> <user3> &#8230; &#8211; creates a groupchat with users, use chat_add_user to add more users
      </li>
      <li>
        <strong>chat_set_photo</strong> <chat> <photo-file-name> &#8211; sets group chat photo. Same limits as for profile photos.
      </li>
    </ul>
    
    <h4>
      <a id="user-content-search" class="anchor" href="https://github.com/vysheng/tg/blob/master/README.md#search"></a>Search
    </h4>
    
    <ul class="task-list">
      <li>
        <strong>search</strong> <peer> pattern &#8211; searches pattern in messages with peer
      </li>
      <li>
        <strong>global_search</strong> pattern &#8211; searches pattern in all messages
      </li>
    </ul>
    
    <h4>
      <a id="user-content-secret-chat" class="anchor" href="https://github.com/vysheng/tg/blob/master/README.md#secret-chat"></a>Secret chat
    </h4>
    
    <ul class="task-list">
      <li>
        <strong>create_secret_chat</strong> <user> &#8211; creates secret chat with this user
      </li>
      <li>
        <strong>visualize_key</strong> <secret_chat> &#8211; prints visualization of encryption key. You should compare it to your partner&#8217;s one
      </li>
      <li>
        <strong>set_ttl</strong> <secret_chat> <ttl> &#8211; sets ttl to secret chat. Though client does ignore it, client on other end can make use of it
      </li>
      <li>
        <strong>accept_secret_chat</strong> <secret_chat> &#8211; manually accept secret chat (only useful when starting with -E key)
      </li>
    </ul>
    
    <h4>
      <a id="user-content-stats-and-various-info" class="anchor" href="https://github.com/vysheng/tg/blob/master/README.md#stats-and-various-info"></a>Stats and various info
    </h4>
    
    <ul class="task-list">
      <li>
        <strong>user_info</strong> <user> &#8211; prints info about user
      </li>
      <li>
        <strong>history</strong> <peer> [limit] &#8211; prints history (and marks it as read). Default limit = 40
      </li>
      <li>
        <strong>dialog_list</strong> &#8211; prints info about your dialogs
      </li>
      <li>
        <strong>contact_list</strong> &#8211; prints info about users in your contact list
      </li>
      <li>
        <strong>suggested_contacts</strong> &#8211; print info about contacts, you have max common friends
      </li>
      <li>
        <strong>stats</strong> &#8211; just for debugging
      </li>
      <li>
        <strong>show_license</strong> &#8211; prints contents of GPLv2
      </li>
      <li>
        <strong>help</strong> &#8211; prints this help
      </li>
    </ul>
    
    <h4>
      <a id="user-content-card" class="anchor" href="https://github.com/vysheng/tg/blob/master/README.md#card"></a>Card
    </h4>
    
    <ul class="task-list">
      <li>
        <strong>export_card</strong> &#8211; print your &#8216;card&#8217; that anyone can later use to import your contact
      </li>
      <li>
        <strong>import_card</strong> <card> &#8211; gets user by card. You can write messages to him after that.
      </li>
    </ul>
    
    <h4>
      <a id="user-content-other" class="anchor" href="https://github.com/vysheng/tg/blob/master/README.md#other"></a>Other
    </h4>
    
    <ul class="task-list">
      <li>
        <strong>quit</strong> &#8211; quit
      </li>
      <li>
        <strong>safe_quit</strong> &#8211; wait for all queries to end then quit
      </li>
    </ul>
    
    <p>
      </div> </div><!-- end div.wp-spoiler -->
      
      <hr />
      
      <p>
        <strong>Refrensi: </strong><span style="color: #999999;"><a style="color: #999999;" title="https://github.com/vysheng/tg/blob/master/README.md" href="https://github.com/vysheng/tg/blob/master/README.md" target="_blank">https://github.com/vysheng/tg/blob/master/README.md</a> | <a style="color: #999999;" title="https://travis-ci.org/vysheng/tg/jobs/47005703" href="https://travis-ci.org/vysheng/tg/jobs/47005703" target="_blank">https://travis-ci.org/vysheng/tg/jobs/47005703</a> | <a style="color: #999999;" title="http://blog.erphidi.com/2014/04/cara-install-telegram-cli-di-linux-mint.html " href="http://blog.erphidi.com/2014/04/cara-install-telegram-cli-di-linux-mint.html" target="_blank">http://blog.erphidi.com/2014/04/cara-install-telegram-cli-di-linux-mint.html<strong><br /> </strong></a></span>
      </p>