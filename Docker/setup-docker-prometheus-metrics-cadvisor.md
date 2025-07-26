# Setup Prometheus exporter for Docker metrics

[cAdvisor](https://github.com/google/cadvisor) is a running daemon that collects,
aggregates, processes, and exports information about running containers.
Specifically, for each container it keeps resource isolation parameters,
historical resource usage, histograms of complete historical resource usage and network statistics.
This data is exported by container and machine-wide

Instruction below was prepared for Ubuntu Server.

1. Run cAdvisor in Docker container. Sample docker compose file:

```YAML
services:
  cAdvisor:
    privileged: true
    image: gcr.io/cadvisor/cadvisor:v0.49.1
    container_name: cAdvisor_metrics
    restart: unless-stopped
    devices:
      - /dev/kmsg
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/snap/docker/common/var-lib-docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    ports:
      - 8080:8080
```

2. Update `daemon.json` file by adding key like below:

```JSON
{
    "metrics-addr" : "0.0.0.1:9323",
}
```

3. Restart Docker service

```shell
systemctl restart snap.docker.dockerd.service
```

In case service was not found you can use command below to find actual name:

```shell
sudo systemctl list-units --type=service
```

4. Important links to check:
   - metrics: `<ip_address>:9323/metrics`
   - cAdvisor console: `<ip_address>:8080`
