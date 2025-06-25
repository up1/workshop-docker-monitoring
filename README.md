# Workshop :: Monitoring Docker
* [node-exporter](https://github.com/prometheus/node_exporter)
* [cadvisor](https://github.com/google/cadvisor)
* [prometheus](https://prometheus.io/)
* [grafana](https://grafana.com/)
* [loki](https://grafana.com/docs/loki/latest/send-data/docker-driver/)

## Docker monitoring :: metric data
```
$docker compose up -d node-exporter
$docker compose up -d cadvisor
$docker compose up -d prometheus
$docker compose up -d grafana
$docker compose ps
```

## List of URLs
* cadvisor
  * http://localhost:9323
* prometheus
  * http://localhost:9090
* grafana
  * http://localhost:3000
    * user=admin
    * pass=admin
* List of grafana dashboard
  * https://grafana.com/grafana/dashboards/?search=Docker+monitoring

## Docker logging with Loki
```
$docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions

$docker plugin list
ID             NAME          DESCRIPTION           ENABLED
7f4656145bbb   loki:latest   Loki Logging Driver   true
```

### Start Loki
```
$docker compose up -d loki
$docker compose ps
```

### Config [Docker deamon](https://docs.docker.com/engine/daemon/) to send logs to Loki server
* Linux, regular setup	/etc/docker/daemon.json
* Linux, rootless mode	~/.config/docker/daemon.json
* Windows	C:\ProgramData\docker\config\daemon.json

```
{
  "debug": true,
  "log-driver": "loki",
  "log-opts": {
    "loki-url": "http://localhost:3100/loki/api/v1/push",
    "loki-batch-size": "400"
  }
}
```

### Restart Docker
```
$sudo systemctl restart docker
$sudo systemctl status docker
```

### Open grafana dashboard
* 