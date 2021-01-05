---
id: 912
title: Membuat Instance OpenStack dengan Fixed IP
date: 2018-02-17T06:47:56+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=912
permalink: /membuat-instance-openstack-dengan-fixed-ip/
categories:
  - Linux
  - OpenStack
tags:
  - fixed ip
  - neutron
  - nova
  - OpenStack
  - port
  - static
---
Terkadang kita mungkin memiliki kebutuhan untuk membuat sebuah instance dengan fixed IP (<del datetime="2018-07-13T13:27:27+00:00">statis</del>) tertentu. Untuk melakukan ini kita harus membuat secara manual port neutron. panduan ini bisa dilakukan untuk network External atau Internal

**BUI (Browser User Interface)**

1. Masuk Panel **Admin > Networks > Ports > Klik Network External > Ports**  
[<img class="alignleft size-large wp-image-917" src="http://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static01-1024x555.png" alt="" width="730" height="396" srcset="https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static01-1024x555.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static01-300x163.png 300w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static01-768x417.png 768w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static01-1200x651.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static01.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static01.png)

2. Klik **+Create Port** lalu isi kan bagian yang dibutuhkan sebagai berikut:  
[<img class="alignleft size-large wp-image-921" src="http://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static02-1024x555.png" alt="" width="730" height="396" srcset="https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static02-1024x555.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static02-300x163.png 300w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static02-768x417.png 768w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static02-1200x651.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static02.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static02.png)

3. Membuat Instance seperti biasa, hanya saja pada tab **Networks** di lewat atau di _Unselect_ gunakan **Network Ports** yang sudah kita buat tadi  
[<img class="alignleft size-large wp-image-924" src="http://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static03-1024x555.png" alt="" width="730" height="396" srcset="https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static03-1024x555.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static03-300x163.png 300w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static03-768x417.png 768w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static03-1200x651.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static03.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static03.png)

4. Verifikasi Instance sudah berjalan dengan IP statis yang sudah kita buat tadi  
[<img class="alignleft size-large wp-image-926" src="http://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static04-1024x555.png" alt="" width="730" height="396" srcset="https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static04-1024x555.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static04-300x163.png 300w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static04-768x417.png 768w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static04-1200x651.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static04.png 1366w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2018/02/ip_static04.png)  
**CLI (Command Line Interface)**

Jika kalian lebih terbiasa menggunakan CLI, berikut caranya:

<pre>neutron port-create --fixed-ip subnet_id=SUBNET_ID,ip_address=IP_FROM_POOL --name PORT_NAME NETWORK_ID</pre>

<pre>[root@controller ~(keystone_admin)]# neutron subnet-list
+--------------------------------------+-----------------+----------------------------------+---------------+--------------------------------------------------+
| id                                   | name            | tenant_id                        | cidr          | allocation_pools                                 |
+--------------------------------------+-----------------+----------------------------------+---------------+--------------------------------------------------+
| cfd88ecd-9f98-43b1-9946-a4f691ea697e | subnet-external | 0f21133e62fd4bdb8c2af962047036ec | 10.11.11.0/24 | {"start": "10.11.11.100", "end": "10.11.11.200"} |
+--------------------------------------+-----------------+----------------------------------+---------------+--------------------------------------------------+</pre>

<pre>[root@controller ~(keystone_admin)]# neutron net-list
+--------------------------------------+----------+----------------------------------+----------------------------------------------------+
| id                                   | name     | tenant_id                        | subnets                                            |
+--------------------------------------+----------+----------------------------------+----------------------------------------------------+
| a2709ab0-5fb0-4c7a-b8c7-43f4f43179b8 | external | 0f21133e62fd4bdb8c2af962047036ec | cfd88ecd-9f98-43b1-9946-a4f691ea697e 10.11.11.0/24 |
+--------------------------------------+----------+----------------------------------+----------------------------------------------------+</pre>

<pre>[root@controller ~(keystone_admin)]# neutron port-create --fixed-ip subnet_id=cfd88ecd-9f98-43b1-9946-a4f691ea697e,ip_address=10.11.11.160 --name net_10.11.11.160 a2709ab0-5fb0-4c7a-b8c7-43f4f43179b8

Created a new port:
+-----------------------+-------------------------------------------------------------------------------------+
| Field                 | Value                                                                               |
+-----------------------+-------------------------------------------------------------------------------------+
| admin_state_up        | True                                                                                |
| allowed_address_pairs |                                                                                     |
| binding:host_id       |                                                                                     |
| binding:profile       | {}                                                                                  |
| binding:vif_details   | {}                                                                                  |
| binding:vif_type      | unbound                                                                             |
| binding:vnic_type     | normal                                                                              |
| created_at            | 2018-02-16T23:33:33Z                                                                |
| description           |                                                                                     |
| device_id             |                                                                                     |
| device_owner          |                                                                                     |
| extra_dhcp_opts       |                                                                                     |
| fixed_ips             | {"subnet_id": "cfd88ecd-9f98-43b1-9946-a4f691ea697e", "ip_address": "10.11.11.160"} |
| id                    | 4d8ca200-0aff-4ef0-9115-6bdbe6ee04f1                                                |
| mac_address           | fa:16:3e:21:b7:ef                                                                   |
| name                  | net_10.11.11.160                                                                    |
| network_id            | a2709ab0-5fb0-4c7a-b8c7-43f4f43179b8                                                |
| project_id            | 0f21133e62fd4bdb8c2af962047036ec                                                    |
| revision_number       | 4                                                                                   |
| security_groups       | 4740f9f0-c5d7-4922-9163-31b9e2b97354                                                |
| status                | DOWN                                                                                |
| tags                  |                                                                                     |
| tenant_id             | 0f21133e62fd4bdb8c2af962047036ec                                                    |
| updated_at            | 2018-02-16T23:33:33Z                                                                |
+-----------------------+-------------------------------------------------------------------------------------+</pre>

<pre>[root@controller ~(keystone_admin)]# neutron port-list
+--------------------------------------+------------------+----------------------------------+-------------------+------------------------------------------+
| id                                   | name             | tenant_id                        | mac_address       | fixed_ips                                |
+--------------------------------------+------------------+----------------------------------+-------------------+------------------------------------------+
| 4d8ca200-0aff-4ef0-9115-6bdbe6ee04f1 | net_10.11.11.160 | 0f21133e62fd4bdb8c2af962047036ec | fa:16:3e:21:b7:ef | {"subnet_id": "cfd88ecd-                 |
|                                      |                  |                                  |                   | 9f98-43b1-9946-a4f691ea697e",            |
|                                      |                  |                                  |                   | "ip_address": "10.11.11.160"}            |
| b7925295-79a6-4200-8078-cde16db6aa53 | net_10.11.11.150 | 0f21133e62fd4bdb8c2af962047036ec | fa:16:3e:c3:cf:51 | {"subnet_id": "cfd88ecd-                 |
|                                      |                  |                                  |                   | 9f98-43b1-9946-a4f691ea697e",            |
|                                      |                  |                                  |                   | "ip_address": "10.11.11.150"}            |
| d65239ef-8544-4893-a334-0100e9dcd2d3 |                  | 0f21133e62fd4bdb8c2af962047036ec | fa:16:3e:35:9b:cd | {"subnet_id": "cfd88ecd-                 |
|                                      |                  |                                  |                   | 9f98-43b1-9946-a4f691ea697e",            |
|                                      |                  |                                  |                   | "ip_address": "10.11.11.100"}            |
+--------------------------------------+------------------+----------------------------------+-------------------+------------------------------------------+</pre>

Sekian.