# Setup MicroK8s cluster on Ubuntu

[MicroK8s](https://microk8s.io) is a lightweight, streamlined, and easy-to-install version of Kubernetes
developed by Canonical (the company behind Ubuntu).
It is designed to run on developer workstations, edge devices, IoT, and CI/CD pipelines,
providing a full Kubernetes environment in a minimal footprint.

1. Install MicroK8s cluster

```shell
sudo snap install microk8s --classic
```

2. Add current user to MicroK8s group

```shell
sudo usermod -a -G microk8s "$(whoami)"
sudo chown -R "$(whoami)" ~/.kube
newgrp microk8s
```

3. Check the status while Kubernetes starts

```shell
microk8s status --wait-ready
```

4. Turn on core services

```shell
microk8s enable dashboard
microk8s enable dns
microk8s enable registry
microk8s enable istio
```

5. Enable ingress for cluster

```shell
microk8s enable metallb
```

You will be asked for IP address pool which metallb will be able to use.

```shell
microk8s enable ingress
```

6. Check if ingress was installed successfully

```shell
microk8s.kubectl get all -n ingress
```

7. Install kubectl

```shell
sudo snap install kubectl --classic
```

8. Create config for kubectl

```shell
cd $HOME
cd .kube
microk8s config > config
```

9. Connect microk8s to kubectl

```shell
kubectl config use-context microk8s
```
