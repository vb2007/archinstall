# Setting up SSH client & server

Installing the openssh package

```shell
yay -S openssh
```

Starting the systemd service

```shell
sudo systemctl enable --now sshd
```

Verifying status

```shell
sudo systemctl status sshd
```
