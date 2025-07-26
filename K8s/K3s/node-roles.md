# K3s Node Roles

## Typical Cluster Setups

- Single-node (dev/test)

  - One server node: runs control plane + workloads.

- Small production

  - 3 server nodes (HA, etcd or external DB)
  - 2+ agent nodes (workload-only)

- Edge cluster

  - 1 server node (control plane only)
  - Multiple low-power agents (e.g., Raspberry Pi)

## Server Node

Also called the control plane, the server node:

- Acts as the brain of the cluster.
- Hosts the Kubernetes control plane components, including:
  - _kube-apiserver_: Handles communication between users and the cluster.
  - _kube-controller-manager_: Manages controllers like deployments and replica sets.
  - _kube-scheduler_: Assigns pods to nodes based on resources and policies.
  - _cloud-controller_-manager (optional)
- Runs the datastore:
  - _SQLite_ (default, for single-node or dev)
  - _etcd_, _MySQL_, or _PostgreSQL_ (for production HA)
- Can optionally run workloads (unless set as control-plane only).

You can run multiple servers for **high availability** (HA). **Minimum 3 server nodes recommended for HA to tolerate a failure**.

## Agent Node

- Only responsible for running workloads (pods).
- Runs components like:

  - _kubelet_: Talks to the control plane and manages containers on the node.
  - _kube-proxy_: Maintains network rules on the node.
  - _containerd_: The container runtime.

- Does not run:
  - Any control plane services like API server or scheduler.
