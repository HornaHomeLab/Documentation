# Run K3s cluster on Proxmox

## Provision K3s cluster

1. Navigate to one of the HornaHomeLab repos that represents Proxmox nodes and clone it:

   - [pve-r7](https://github.com/HornaHomeLab/pve-r7)

2. Create a new branch (`git checkout -b "<your_branch_name>"`)

3. In `./terraform` modify a `ubuntu-vms.tf` file to add Vms, which will be a nodes in K3s cluster.

You need to add new entries in `ubuntu_vms` variable, single entry should look like:

```HCL
    "<proxmox_vm_name>" = {
      target_node  = "<node_hosting_vm>"
      vm_desc      = "<vm_description>"
      ip_address   = "<vm_ip_address>"
      cidr_netmask = "24"
      gateway      = "10.0.10.1"
      dns_servers  = ["10.0.10.11", "1.1.1.1"]
      tags         = ["ubuntu", "<k3s_tag>"]
    }
```

- `<proxmox_vm_name>` - VM name from Proxmox perspective
- `<node_hosting_vm>` - Name of the Proxmox Node, where VM will be
- `<vm_description>` - Your description of the VM
- `<k3s_tag>` - Appropriate k3s tag.

K3s cluster must consists of at least one node with server role.
Refer to [K3s Node Roles](/K8s/K3s/node-roles.md) for more information.
To specify which VM should have which role you need to assign appropriate `<k3s_tag>`,
which follows the format:

```
k3s_cluster_<environment_name>_<node_role>
```

- `<environment_name>` - Is the name of the environment, or just the cluster identifier,
  it can be any name, but there should not be another cluster under the same name.

- `<node_role>` - `agent` either `server`.

4. Once your changes are ready create a Pull Request to `main` branch.
   After it will be approved and merged setup process will start automatically.
   You can monitor progress of the provisioning by watching GitHub Actions.

5. When provisioning part is completed you can follow [HashiCorp Vault / Signed SSH certificates](/HashiCorp_Vault/signed-ssh-certificates.md)
   to connect to provisioned VMs.
