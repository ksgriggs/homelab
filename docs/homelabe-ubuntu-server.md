# Ubuntu Server

Notes for installing Ubuntu sever on an old laptop.

## LVM (Logical Volume Manager)

I chose to have Ubuntu use LVM. This adds the install disk and any others that you choose to a volume group. After installation, I noticed my total disk space was not equal to the drive size installed in the laptop. This is because Ubuntu does not use all the space in the volume group. It creates logical volumes and mounts them where needed. The logical volume for the / mount point was only 100 GB. You can increase the size of the logical volumes if needed.

https://documentation.ubuntu.com/server/how-to/storage/manage-logical-volumes/#manage-logical-volumes

## Laptops

This is how you disable hibernate when the lid is closed.
Edit /etc/systemd/logind.conf and modify these values. Make sure they are not commented out.

```
HandleLidSwitch=ignore
LidSwitchIgnoreInhibitied=no
```

Restart logind:

```
sudo service systemd-logind restart
```

## Network Configuration

I gave the laptop a static IP address. I renamed the files in /etc/netplan so they would not be used and created a fila called 01-netcfg.yaml with the following in it.

File: 01-netcfg.yaml

```yaml
network:
  version: 2
  renderer: networkd
  wifis:
    wlp2s0:
      dhcp4: false
      dhcp6: false
      addresses: [192.168.1.101/24]
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
      access-points:
        "<SID GOES HERE>":
          password: "<PASSWORD GOES HERE>"
      routes:
        - to: default
          via: 192.168.1.1
```
