---
id: 1246
title: Egress IP OpenShift 4
date: 2021-12-7T13:47:58+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=1246
permalink: /egress-ip-openshift-4/
categories:
  - Linux
  - OpenShift
tags:
  - OpenShift
  - OCP
  - OKD
  - Egress
---
Cara mengkonfigurasi Egress IP agar semua koneksi menuju eksternal services menggunakan fixed IP address. 





## Environment Testing
- OpenShift - OCP Cluster 4.7
- External services (Web) - 192.168.10.29/24
- Fix IP address untuk Egress IP - 192.168.10.179/24

## External Web server
Pasang paket web server untuk pengujian
```
yum install -y httpd
systemctl start httpd
```
Ubah file index.html
```
cat > /var/www/html/index.html << EOF
OK - from external services
EOF
```
Ujicoba akses ke localhost
```
curl localhost
```

## Container For Testing
Buat project dan deploy container untuk testing
```
oc new-project egress-ip
oc create deployment test-nginx --image quay.io/redhattraining/hello-world-nginx
oc get deploy
```

Cek Pod berjalan di Worker mana
```
oc get pod -o wide
```
```
NAME                          READY   STATUS    RESTARTS   AGE    IP             NODE                             NOMINATED NODE   READINESS GATES
test-nginx-86856dbc5c-dpb22   1/1     Running   0          2m5s   10.129.4.175   worker01.roar.lab   <none>           <none>
```
Periksa IP worker dimana pod berjalan
```
oc get po -o custom-columns=NAME:.spec.containers[0].name,NODE:.spec.nodeName,POD_IP:.status.podIP,HOST_IP:.status.hostIP
```
```
NAME                NODE                             POD_IP         HOST_IP
hello-world-nginx   worker01.roar.lab   10.129.4.175   192.168.10.16
```
## Testing Connecting to External Web Server
Panggil external services via container
```
oc rsh deploy/test-nginx curl http://192.168.10.29
OK - from external services
```
Periksa log di node Web server
```
tail -f /var/log/httpd/access_log
192.168.10.16 - - [07/Dec/2021:05:31:58 +0000] "GET / HTTP/1.1" 200 28 "-" "curl/7.61.1"
```
## Basic Egress IP Test
Set IP range ke node worker01
```
oc patch hostsubnet worker01 --type=merge -p '{"egressCIDRs": ["192.168.50.179/24"]}'
```
Set Egress IP untuk namespace
```
oc patch netnamespace egress-ip --type=merge -p '{"egressIPs": ['192.168.50.179']}'
```
Cek apakah Egress IP sudah terconfig dengan baik atau belum
```
ssh core@192.168.10.16 ip a show dev ens3
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:3e:be:20 brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.16/24 brd 192.168.10.255 scope global dynamic noprefixroute ens3
       valid_lft 417sec preferred_lft 417sec
    inet 192.168.10.179/24 brd 192.168.10.255 scope global secondary ens3:eip
       valid_lft forever preferred_lft forever
    inet6 fe80::a64b:56ee:b9a3:93cb/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
Lakukan uji test lagi 
```
oc rsh deploy/test-nginx curl http://192.168.10.29
OK - from external services
```
Periksa kembali log di node Web server
```
tail -f /var/log/httpd/access_log
192.168.10.179 - - [07/Dec/2021:05:40:58 +0000] "GET / HTTP/1.1" 200 28 "-" "curl/7.61.1"
```
## Failover Test
Karena Egress IP hanya jalan di worker01, lakukan ujicoba dengan menshutdown node worker01
```
oc get nodes
NAME                STATUS                      ROLES    AGE  VERSION
master01.roar.lab   Ready,SchedulingDisabled    master   2d   v1.20.0+bafe72f
master02.roar.lab   Ready,SchedulingDisabled    master   2d   v1.20.0+bafe72f
master03.roar.lab   Ready,SchedulingDisabled    master   2d   v1.20.0+bafe72f
worker01.roar.lab   NotReady                    worker   2d   v1.20.0+bafe72f
worker02.roar.lab   Ready                       worker   2d   v1.20.0+bafe72f
worker03.roar.lab   Ready                       worker   2d   v1.20.0+bafe72f
worker04.roar.lab   Ready                       worker   2d   v1.20.0+bafe72f
```
Sekarang mari kita jalankan tes lagi
```
oc rsh deploy/test-nginx curl http://192.168.10.29
```
Tes akan gagal karena tidak ada network yang dapat melakukan request
## Failover with 2+ nodes
Untuk mengatasi itu, set Egress range juga di node worker yang lain
```
oc patch hostsubnet worker02 --type=merge -p '{"egressCIDRs": ["192.168.50.179/24"]}'
oc patch hostsubnet worker03 --type=merge -p '{"egressCIDRs": ["192.168.50.179/24"]}'
```
Cek dan pastikan Egress CIDRs dinode worker
```
oc get hostsubnet
NAME                HOST                HOST IP         SUBNET          EGRESS CIDRS            EGRESS IPS
master01.roar.lab   master01.roar.lab   192.168.10.4    10.128.0.0/23                           
master02.roar.lab   master02.roar.lab   192.168.10.5    10.129.0.0/23
master03.roar.lab   master03.roar.lab   192.168.10.6    10.130.0.0/23
worker01.roar.lab   worker01.roar.lab   192.168.10.16   10.129.4.0/23   ["192.168.10.179/24"]   ["192.168.10.179"]
worker02.roar.lab   worker02.roar.lab   192.168.10.17   10.128.2.0/23   ["192.168.10.179/24"]
worker03.roar.lab   worker03.roar.lab   192.168.10.18   10.129.2.0/23   ["192.168.10.179/24"]
```
Uji coba memanggil web server sambil mematikan worker01 dimana Egress IP bound, harusnya Egress IPs akan pindah ke node lain.
```
while true; do oc rsh deploy/test-nginx curl http://192.168.10.29 ;sleep 1; done
```
```
oc get hostsubnet
NAME                HOST                HOST IP         SUBNET          EGRESS CIDRS            EGRESS IPS
master01.roar.lab   master01.roar.lab   192.168.10.4    10.128.0.0/23                           
master02.roar.lab   master02.roar.lab   192.168.10.5    10.129.0.0/23
master03.roar.lab   master03.roar.lab   192.168.10.6    10.130.0.0/23
worker01.roar.lab   worker01.roar.lab   192.168.10.16   10.129.4.0/23   ["192.168.10.179/24"]   
worker02.roar.lab   worker02.roar.lab   192.168.10.17   10.128.2.0/23   ["192.168.10.179/24"]   ["192.168.10.179"]
worker03.roar.lab   worker03.roar.lab   192.168.10.18   10.129.2.0/23   ["192.168.10.179/24"]
```
Cek Egress IP di node worker02
```
core@192.168.10.17 ip a show dev ens3
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:ba:05:34 brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.17/24 brd 192.168.10.255 scope global dynamic noprefixroute ens3
       valid_lft 526sec preferred_lft 526sec
    inet 192.168.10.179/24 brd 192.168.10.255 scope global secondary ens3:eip
       valid_lft forever preferred_lft forever
    inet6 fe80::a303:2c26:5033:c7d4/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
