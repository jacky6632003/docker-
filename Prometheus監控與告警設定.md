# Prometheus監控與告警設定
```bash
# 建立 prometheus 監控站台
docker run --name prometheus -p 9090:9090 -v /data/prometheus-data:/prometheus-data --restart always -d prom/prometheus --config.file=/prometheus-data/prometheus.yml

# 編譯規則檔,產生的檔案,如：alert.rules.yml
docker exec -it prometheus promtool update rules /prometheus-data/alert.rules

# 建立告警管理員 prometheus alertmanager 
docker run --name alertmanager -p 9093:9093 -v /data/prometheus-alertmanager:/prometheus-alertmanager -d prom/alertmanager  --config.file=/prometheus-alertmanager/alertmanager.yml

```

# Prometheus儲存資料說明
資料預設儲存在本機 /data,並且於15d後刪除舊資料
但是在啟動Docker的時候也可以額外做一些設定
```bash
# 用来設定資料儲存時間，168h0m0s表示24*7小时，即1週
XXXXX -storage.local.retention 168h0m0s \
--storage.tsdb.retention 30d

# 等待寫入磁碟的最大chunks數,超過大小寫入會被限制,建議設定為storage.local.memory-chunks的50%
-storage.local.max-chunks-to-persist 3024288 \

# prometheus記憶中保留最大的chunks數，預設1048576，即为1G大小
-storage.local.memory-chunks=50502740 \

# 增加分配的mutexes
-storage.local.num-fingerprint-mutexes=300960

# 寫入資料後何時同步到硬碟 'never', 'always', 'adaptive'
-storage.local.series-sync-strategy

# 間隔多久進行一次checkpoint
-storage.local.checkpoint-interval

```

# 參考連結
https://mkezz.wordpress.com/2017/11/13/prometheus-command-line-flags-in-docker-service/


```bash
[root@srvdocker-t system]# tar -C /usr/local -zvxf node_exporter-0.15.2.linux-amd64.tar.gz
[root@srvdocker-t system]# vim /etc/systemd/system/exporter.service
[root@srvdocker-t system]# systemctl enable exporter.service
Created symlink from /etc/systemd/system/multi-user.target.wants/exporter.service to /usr/lib/systemd/system/exporter.service.
[root@srvdocker-t system]# systemctl start exporter.service
[root@srvdocker-t system]# systemctl daemon-reload
```

exporter.service 內容

```sh
[Unit]
Description=node exporter

[Service]
Type=simple
ExecStart=/usr/local/node_exporter-0.15.2.linux-amd64/node_exporter --web.listen-address=:9111

[Install]
WantedBy=multi-user.target
```

