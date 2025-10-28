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
