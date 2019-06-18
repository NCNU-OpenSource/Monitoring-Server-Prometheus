# Monitoring-Server

## 專案簡介
- 隨著部署/管理 伺服器的增加，對於一天只有24小時的猴子，無法實時知道伺服器的狀態，但隨著猴子的進化，還有更強大的猴子開發了強大的工具——「Prometheus」、「Grafana」。
    - - [x] 可針監控Service
    - - [x] 可監控伺服器狀況
    - - [x] 視覺化呈現監控結果
    - ……
- 手機不離手，搭配IM的使用，猴子的可用時間又多了一點！
    - - [x] Telegram Bot


## 安裝過程

### Prometheus
1. 下載並解壓縮 Prometheus 2.10
```shell=
wget https://github.com/prometheus/prometheus/releases/download/v2.10.0/prometheus-2.10.0.linux-amd64.tar.gz

tar xvfz prometheus-2.10.0.linux-amd64.tar.gz
cd prometheus-2.10.0.linux-amd64
```

2. 設定 prometheus.yml
- `可以不用設定`
```shell=

```
3. 執行 prometheus (背景執行)
```shell=
./prometheus --config.file=prometheus.yml &
```

### 監控 Node-exporter

### Grafana

1. 下載並安裝 Grafana
```shell=
wget https://dl.grafana.com/oss/release/grafana_6.2.2_amd64.deb 

sudo dpkg -i grafana_6.2.2_amd64.deb
```

:::info
- Installs configuration file to `/etc/grafana/grafana.ini`

- The default configuration sets the log file at `/var/log/grafana/grafana.log`

- The default configuration specifies an sqlite3 db at `/var/lib/grafana/grafana.db`
:::

2. 啟動 Grafana
```shell=
sudo service grafana-server start
```

3. 將 Grafana 設置為開機時啟動
```shell=
sudo update-rc.d grafana-server defaults
```

### PushGateWay

1. 安裝
```shell=
wget https://github.com/prometheus/pushgateway/releases/download/v0.8.0/pushgateway-0.8.0.linux-amd64.tar.gz
tar -xvfz pushgateway-0.8.0.linux-amd64.tar.gz
cd pushgateway-0.8.0.linux-amd64
```
2. 啟動(背景執行)
```shell=
./pushgateway & 
```

3. 寫一個 bash 上傳 cpu / memory 資訊

#### 調整檔案權限
```shell=
touch {fileName} 
chmod u+x {fileName} 
vim {fileName} 
```


```bash=
#!/bin/bash
z=$(ps aux)
while read -r z
do
   var=$var$(awk '{print "cpu_usage{process=\""$11"\", pid=\""$2"\"}", $3z}');
done <<< "$z"
curl -X POST -H  "Content-Type: text/plain" --data "$var
" http://localhost:9091/metrics/job/cpu/instance/main_server
```

```bash=
#!/bin/bash
z=$(ps aux)
while read -r z
do
   var=$var$(awk '{print "memory_usage{process=\""$11"\", pid=\""$2"\"}", $4z}');
done <<< "$z"
curl -X POST -H  "Content-Type: text/plain" --data "$var
" http://localhost:9091/metrics/job/memory/instance/main_server
```

## 分工
- [李禹叡]()：PromQL
- [王俊傑]()：部署環境及測試
- [覃融亮]()：

## Reference

[Prometheuse 官網](https://prometheus.io)
- `Offical`

[K8s + Prometheus](https://tpu.thinkpower.com.tw/tpu/articleDetails/992)
- `基礎概念`

[Prometheus 配置](https://songjiayang.gitbooks.io/prometheus/content/configuration/global.html)

[K8s + Prometheus](https://medium.com/getamis/kubernetes-operators-prometheus-3584edd72275)
- `基礎概念`

[Node.js + Prometheus](https://medium.com/the-node-js-collection/node-js-performance-monitoring-with-prometheus-c3d50c2d5608)
- `Node.js`、`監控 HTTP`

[Prometheus + node_exporter + grafana + mail notify](https://blog.51cto.com/youerning/2050543)
- `安裝教學 2017`、`不美觀 Mail`

[Prometheus + node_exporter + grafana](https://site-optimize-note.tk/伺服器效能監控prometheus-與-grafana-安裝教學/)
- `安裝教學 2018`、`有系列`

[IBM - Prometheus 入门与实践](https://www.ibm.com/developerworks/cn/cloud/library/cl-lo-prometheus-getting-started-and-practice/index.html)


