---
id: 1088
title: Monitoring OpenStack Instances with Service Discovery Prometheus + Grafana
date: 2018-12-23T21:03:58+07:00
author: ary
layout: post
guid: http://blog.aryulianto.com/?p=1088
permalink: /monitoring-openstack-instances-with-service-discovery-prometheus-grafana/
categories:
  - Linux
  - OpenStack
tags:
  - Grafana
  - Instances
  - Monitoring
  - nova
  - OpenStack
  - Prometheus
---
Selain bisa men_define_ target secara _static_, Prometheus juga mendukung konfigurasi secara _dynamically_ menggunakan _**<a href="https://prometheus.io/docs/prometheus/latest/configuration/configuration/#openstack_sd_config" rel="noopener" target="_blank">service discovery.</a>**_  
Salah satunya Prometheus dapat melakukan _query_ ke Nova API untuk me-list seluruh Instances di OpenStack sebagai target untuk dimonitoring.

Kebutuhan:  
&#8211; [Prometheus](https://prometheus.io)  
&#8211; [Grafana](https://grafana.com/)  
&#8211; [Node Exporter](https://prometheus.io/docs/guides/node-exporter/)  
&#8211; [OpenStack](https://www.openstack.org/) RC / Credential

Langkah-langkah:

**I. Prometheus Server**

1. Update server dan pasang paket pendukung

<pre>yum -y update
yum -y install vim
</pre>

2. Unduh Prometheus Server

<pre>curl -LO "https://github.com/prometheus/prometheus/releases/download/v2.5.0/prometheus-2.5.0.linux-amd64.tar.gz"
tar xvfz nxvfz prometheus-2.5.0.linux-amd64.tar.gz -C /opt/ && mv prometheus-2.5.0.linux-amd64 prometheus-server
cd /opt/prometheus-server/
</pre>

3. Sunting berkas konfigurasi, sesuaikan konfigurasi yang diinginkan

<pre>vim config.yml

# my global config

global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=` to any timeseries scraped from this config.
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']
  - job_name: 'openstack-sd'
    openstack_sd_configs:
      - identity_endpoint: https://url-keystone-server:443/v3
        username: &lt;username>
        project_id: &lt;project_id>
        password: &lt;secret>
        role: 
        region: 
        domain_name: 
        port: 9100
</pre>

4. Verifikasi berkas config Prometheus server pastikan _SUCCESS_

<pre>./promtool check config config.yml</pre>

5. Jalankan Prometheus server sebagai service

<pre>vim /etc/systemd/system/prometheus-server.service

[Unit]
Description=Prometheus Server

[Service]
User=root
ExecStart=/opt/prometheus-server/prometheus --config.file=/opt/prometheus-server/config.yml --web.external-url=http://ip-server:9090/

[Install]
WantedBy=default.target
</pre>

6. Jalankan service Prometheus Server

<pre>systemctl daemon-reload
systemctl enable prometheus-server.service
systemctl start prometheus-server.service
</pre>

7. Verifikasi bahwa service Prometheus sudah berjalan

<pre>systemctl status prometheus-server.service
ss -tulpn |grep 9090
</pre>

**II. Grafana**  
1. Pasang Grafana

<pre>yum -y install https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-5.1.4-1.x86_64.rpm
/sbin/chkconfig --add grafana-server
</pre>

2. Jalankan service Grafana

<pre>systemctl enable grafana-server.service
systemctl start grafana-server.service
systemctl status grafana-server.service
</pre>

3. Akses Grafana Dashboard http://ip-server:3000  
`<br />
– Login dengan usename dan passsword admin/admin<br />
– Klik 'Add Datasource'<br />
– Name: Prometheus, Type: Prometheus<br />
– Http settings: http://localhost:9090<br />
– Klik 'Save and Test'.<br />
Pastikan hasilnya 'success' dan 'datasource added'<br />
` 

4. Buat dashboard atau bisa juga unduh di <a href="https://grafana.com/dashboards" rel="noopener" target="_blank">https://grafana.com/dashboards</a> sesuai dengan kebutuhan anda, sebagai contoh:  
[<img src="http://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-1024x500.png" alt="" width="730" height="356" class="alignnone size-large wp-image-1105" srcset="https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-1024x500.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-300x147.png 300w, https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-768x375.png 768w, https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-1200x586.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard.png 1365w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard.png)

[<img src="http://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-2-1024x500.png" alt="" width="730" height="356" class="alignnone size-large wp-image-1120" srcset="https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-2-1024x500.png 1024w, https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-2-300x147.png 300w, https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-2-768x375.png 768w, https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-2-1200x586.png 1200w, https://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-2.png 1365w" sizes="(max-width: 730px) 100vw, 730px" />](http://blog.aryulianto.com/wp-content/uploads/2018/12/grafana-dashboard-2.png)

**III. Node Exporter**

1. Pasang Node Exporter ditiap Instance yang ingin anda monitoring

<pre>#!/bin/bash

sudo -i
curl -LO "https://github.com/prometheus/node_exporter/releases/download/v0.17.0/node_exporter-0.17.0.linux-amd64.tar.gz"
tar xvfz node_exporter-0.17.0.linux-amd64.tar.gz -C /opt/

cat &lt;&lt; EOF >/etc/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter

[Service]
User=root
ExecStart=/opt/node_exporter-0.17.0.linux-amd64/node_exporter

[Install]
WantedBy=default.target
EOF

systemctl daemon-reload
systemctl enable node_exporter.service
systemctl start node_exporter.service
systemctl status node_exporter.service
rm -rf node_exporter-0.17.0.linux-amd64.tar.gz
</pre>

Sekarang saat ada Instances baru anda hanya perlu memasang atau menambahkan Node Exporter (bisa juga dipasang waktu _create_ Instance di _Customization Script_) dan secara otomatis Prometheus server akan men-scrapenya dan _viola!_ seluruh Instances bisa dimonitoring di dashboard Grafana sekarang 😀 

* * *

**Referensi:**  
<a href="https://medium.com/@pasquier.simon/monitoring-your-openstack-instances-with-prometheus-a7ff4324db6c" rel="noopener" target="_blank">https://medium.com/@pasquier.simon/monitoring-your-openstack-instances-with-prometheus-a7ff4324db6c</a>