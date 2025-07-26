# Add self signed root certificate to Synology DSM

Synology DSM by default does not trust any docker registry without valid certificate.
To enable downloading docker images from local registry we need to add root cert to trusted ones in Synology OS

1. Download root certificate locally.
2. Upload it to any Synology share.
3. Connect via SSH to Synology Disk Station
4. Copy uploaded cert to required `/etc/ssl/certs`, using:

```shell
cp ./root-ca.crt /etc/ssl/certs/<your_domain>root-ca.crt
```

5. Login as a root: `sudo -i`

6. Create a “hash-named” symlink, using:

```shell
ln -s /etc/ssl/certs/<your_domain>root-ca.crt /etc/ssl/certs/`openssl x509 -hash -noout -in /etc/ssl/certs/<your_domain>root-ca.crt`.1
```

7. Reboot Synology DSM to apply changes
