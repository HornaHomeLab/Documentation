# Signed SSH certificates

Signed SSH certificates are a secure and scalable alternative
to traditional SSH key-based authentication.
Instead of relying solely on manually managed public key files (~/.ssh/authorized_keys),
SSH certificates introduce a trusted Certificate Authority (CA) to validate user and host identities.

## Configure target host to work with SSH Certificates

After provisioning necessary Vault roles follow instructions below:

1. Get SSH CA Public key and save it on **target** machine in `/etc/ssh/trusted-user-ca-keys.pem`

```shell
vault read -field=public_key ssh/config/ca > /etc/ssh/trusted-user-ca-keys.pem
```

2. Modify `/etc/ssh/sshd_config`, by adding following line to it:

```
TrustedUserCAKeys /etc/ssh/trusted-user-ca-keys.pem
```

3. Restart SSH service

```shell
sudo systemctl restart ssh
```

## Sign your Key to access target machine

1. Generate SSH key pair if you do not have one.

```shell
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa
```

2. Sign your SSH Key with Certificate.

Ready to use:

```shell
vault write -field=signed_key ssh/sign/proxmox  \
    public_key=@$HOME/.ssh/id_rsa.pub \
    valid_principals=proxmox \
    > ./signed-cert.pub
```

3. Connect to target machine

Ready to use:

```shell
ssh proxmox@10.0.10.100 -i ./signed-cert.pub -i ~/.ssh/id_rsa
```

4. (Optional) Check signed cert details

```shell
ssh-keygen -Lf ./signed-cert.pu
```
