# Ubuntu Server

## Set Static IP via CLI

```bash
nano /etc/netplan/00-installer-config.yaml
```

``` yaml
network:
  ethernets:
    ens160:
      addresses: 
      - 10.0.0.11/24
      routes:
        - to: default
          via: 10.0.0.1
      nameservers:
        addresses:
        - 10.0.0.1
        - 1.1.1.1
  version: 2
```

``` shell
netplan apply
```

## NFS Share Automount

``` shell
apt install nfs-common
mount -t nfs4 -o proto=tcp,port=2049 10.0.0.10:/volume1/downloads /downloads
mount -t nfs4 -o proto=tcp,port=2049 10.0.0.11:/volume2/media /media
nano /etc/fstab
```

Content:

``` shell
10.0.0.10:/volume1/downloads /downloads nfs4 _netdev,auto 0 0
10.0.0.11:/volume2/media /media nfs4 _netdev,auto 0 0
```

Reference:

https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-20-04

https://help.ubuntu.com/community/NFSv4Howto