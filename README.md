                                                 ** prometheus && grafana 安装文档**

**1.安装prometheus**

cd /data

mkdir prometheus

wget https://github.com/prometheus/prometheus/releases/download/v2.13.0/prometheus-2.13.0.linux-amd64.tar.gz

tar zxvf prometheus-2.13.0.linux-amd64.tar.gz

mv prometheus-2.13.0.linux-amd64 prometheus

vi /usr/lib/systemd/system/prometheus.service

[unit]

Description=Prometheus Server

Documentation=https://prometheus.io/docs/introduction/overview/

After=network.target

[Service]

User=root

Group=root

Restart=on-failure

WorkingDirectory=/data/prometheus/prometheus

ExecStart=/data/prometheus/prometheus/prometheus \

--config.file=/data/prometheus/prometheus/prometheus.yml \

ExecReload=/bin/kill -HUP $MAINPID

[Install]

WantedBy=multi-user.target

======================================================================================================================

systemctl daemon-reload

systemctl enable prometheus

systemctl start prometheus


**2.安装grafana**

wget https://dl.grafana.com/oss/release/grafana-6.0.2-1.x86_64.rpm

yum install  grafana-6.0.2-1.x86_64.rpm

systemctl daemon-reload

systemctl enable grafana

systemctl start grafana

1.安装pie插件

官网：https://grafana.net/plugins/grafana-piechart-panel  

grafana-cli plugins install grafana-piechart-panel
2.重启grafana-server

3.添加饼图


**3.安装alertmanager**

cd /data/prometheus

wget https://github.com/prometheus/alertmanager/releases/download/v0.17.0/alertmanager-0.17.0.linux-amd64.tar.gz

tar zxvf alertmanager-0.17.0.linux-amd64.tar.gz

mv alertmanager-0.18.0.linux-amd64 alertmanager-0.17.0

vi /usr/lib/systemd/system/alertmanager.service 

[Unit]
Description=Alertmanager
After=network-online.target

[Service]
Restart=on-failure
ExecStart=/data/prometheus/alertmanager-0.17.0/alertmanager --config.file=/data/prometheus/alertmanager-0.17.0/alertmanager.yml

[Install]
WantedBy=multi-user.target

======================================================================================================================

systemctl daemon-reload

systemctl enable alertmanager

systemctl start alertmanager

**4.安装node_exporter**

cd /data

wget https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz

tar zxvf node_exporter-0.18.1.linux-amd64.tar.gz

mv node_exporter-0.18.1.linux-amd64 node_exporter

vi /usr/lib/systemd/system/node_exporter.service 

[Unit]

Description=node_exporter

After=network.target

[Service]

Type=simple

User=root

ExecStart=/data/node_exporter/node_exporter

Restart=on-failure

[Install]



WantedBy=multi-user.target

=======================================================================================================================

systemctl daemon-reload

systemctl enable node_exporter

systemctl start node_exporter

**5.安装blackbox_exporter**

cd /data

wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.16.0/blackbox_exporter-0.16.0.linux-amd64.tar.gz

tar zxvf blackbox_exporter-0.16.0.linux-amd64.tar.gz

mv blackbox_exporter-0.16.0.linux-amd64  blackbox_exporter-0.16.0

vi /usr/lib/systemd/system/blackbox_exporter.service

[Unit]
Description=blackbox_exporter
After=network.target

[Service]
User=root
Type=simple
ExecStart=/data/blackbox_exporter-0.16.0/blackbox_exporter --config.file=/data/blackbox_exporter-0.16.0/blackbox.yml
Restart=on-failure

[Install]
WantedBy=multi-user.target

======================================================================================================================

systemctl daemon-reload

systemctl enable blackbox_exporter

systemctl start blackbox_exporter

**6.webhook安装**

vi /usr/lib/systemd/system/webhook.service

[Unit]
Description=webhook
After=network.target

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/bin/python /data/webhook/webhook.py

[Install]
WantedBy=multi-user.target

======================================================================================================================

systemctl daemon-reload

systemctl enable webhook.service

systemctl start webhook.service

======================================================================================================================

**7.redis_exporter安装**

cd /opt

wget https://github.com/oliver006/redis_exporter/releases/download/v1.3.5/redis_exporter-v1.3.5.linux-amd64.tar.gz

tar zxvf redis_exporter-v1.3.5.linux-amd64.tar.gz

mv redis_exporter-v1.3.5.linux-amd64.tar.gz redis_exporter

vi /usr/lib/systemd/system/redis_exporter.service


[Unit]

Description=redis_exporter

Documentation=https://prometheus.io/

After=network.target


[Service]

Type=simple

User=root

ExecStart=/opt/redis_exporter/redis_exporter -redis.addr 172.18.230.94:3306  -redis.password vi6SaU925f

Restart=on-failure


[Install]

WantedBy=multi-user.target

======================================================================================================================

systemctl daemon-reload

systemctl enable redis_exporter.service

systemctl start redis_exporter.service


**8.mysqld_exporter安装**

cd /opt

wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.12.1/mysqld_exporter-0.12.1.linux-amd64.tar.gz

tar zxvf mysqld_exporter-0.12.1.linux-amd64.tar.gz

mv mysqld_exporter-0.12.1.linux-amd64 mysqld_exporter

vi /usr/lib/systemd/system/mysqld_exporter.service


[Unit]

Description=mysqld_exporter

Documentation=https://prometheus.io/

After=network.target


[Service]

Type=simple

User=root

ExecStart=/opt/mysqld_exporter/mysqld_exporter --config.my-cnf=/opt/mysqld_exporter/.my.cnf

Restart=on-failure


[Install]

WantedBy=multi-user.target

======================================================================================================================

systemctl daemon-reload

systemctl enable mysqld_exporter.service 

systemctl start mysqld_exporter.service 
