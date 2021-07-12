# LogManagement
## Download Loki File
```
wget https://raw.githubusercontent.com/grafana/loki/v2.2.1/cmd/loki/loki-local-config.yaml -O loki-config.yaml
```
## Create Loki Service
```
docker run -d --name loki -v $(pwd):/mnt/config -p 3100:3100 grafana/loki:2.2.1 -config.file=/mnt/config/loki-config.yaml
```
# Download promtail Config
```
wget https://raw.githubusercontent.com/grafana/loki/v2.2.1/cmd/promtail/promtail-docker-config.yaml -O promtail-config.yaml
```
# How to find private IP
```
hostname -I
```
# Update File promtail-config.yaml [IPOFMACHINE]
```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://[IPOFMACHINE]:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - [IPOFMACHINE]
    labels:
      job: varlogs
      __path__: /var/log/*log
```
## create Promtail Service
```
docker run -d --name promtail -v $(pwd):/mnt/config -v /var/log:/var/log grafana/promtail:2.2.1 -config.file=/mnt/config/promtail-config.yaml
```
# Create Grafana Service
```
docker run -d --name grafana -p 3000:3000 --name grafana grafana/grafana:6.5.0
```

### FLOW
 - Client with Promtail
 - Promtail to Loki
 - Loki to Grafana

License
----
harish.chander@hotmail.com
