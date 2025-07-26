# Setup Grafana Loki plugin for Docker

Grafana Loki officially supports a Docker plugin that will read logs from Docker containers and ship them to Loki.

## Install

1. Install loki driver

```shell
docker plugin install grafana/loki-docker-driver:3.3.2 --alias loki --grant-all-permissions
```

2. Adjust daemon.json

```JSON
{
    "log-level": "debug",
    "log-driver": "loki",
    "log-opts": {
        "loki-url": "http://<loki_address>:<loki_port>/loki/api/v1/push",
        "loki-batch-size": "400"
    }
}
```

- `log-level` - messages log level required to be sent to Loki
- `log-driver` - must match the alias specified in plugin installation
- `loki-url` - full url with endpoint path to sent data to loki

## Upgrade

```shell
docker plugin disable loki --force
docker plugin upgrade loki grafana/loki-docker-driver:3.3.2 --grant-all-permissions
docker plugin enable loki
systemctl restart docker
```

## Uninstall

```shell
docker plugin disable loki --force
docker plugin rm loki
```
