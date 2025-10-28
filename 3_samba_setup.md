# Connecting to local samba shares

## Initial setup

First, install a tool for mounting SMB/CIFS shares

```shell
sudo pacman -S cifs-utils
```

## Creating a universal username & password file

Create the credentials file:

```shell
sudo mkdir -p /etc/samba/
sudo nano /etc/samba/credentials
```

Enter relevant values to the credentials file:

```shell
username=your_samba_username
password=your_samba_password
```

Set permissions (only allow root to read it):

```shell
sudo chmod 600 /etc/samba/credentials
```

## Auto-mounting on boot

Creating a mount point:

```shell
sudo mkdir -p /mnt/share1
```

Adding automount to fstab (open the fstab file):

```shell
sudo nano /etc/fstab
```

Enter the relevant mount config:

```shell
//server-ip/share-name /mnt/share1 cifs credentials=/etc/samba/credentials,noauto,x-systemd.automount,x-systemd.idle-timeout=1min,uid=1000,gid=1000,iocharset=utf8 0 0
```

Reloading systemd & testing:

```shell
sudo systemctl daemon-reload
sudo systemctl restart remote-fs.target
```

You should be able to see the mounted shared folder.
