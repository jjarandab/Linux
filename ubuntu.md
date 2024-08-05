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

## Disk Cleanup

Docker cleanup (make sure all containers are running)

```bash
docker system prune -a
```

Delete previous kernel vesions

```bash
dnf remove $(dnf repoquery --installonly --latest-limit=-2 -q)
```

Journal Log Cleanup

```bash
sudo journalctl --vacuum-size=100M
```

DNF Package Cleanup

``` bash
dnf clean packages
```

## LVM

List LVM

```bash
lsblk -o name,size,fstype
```

List Physical Volumes

``` bash
pvscan
pvs
pvdisplay
```

## Drive Automount

``` bash
fdisk -l
blkid
mkdir /data
groupadd data
usermod -aG data USERNAME
chown -R :data /data
nano /etc/fstab
```

Content:

``` bash
UUID=7e98fbe6-a45c-4276-b301-05d1ae8f4db2 /data    auto nosuid,nodev,nofail,x-gvfs-show 0 0
mount -a
```

