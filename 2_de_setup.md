# Installing a desktop environment (and other necessary things)

```shell
sudo pacman -S hyprland waybar neofetch kitty thunar libreoffice openssh wofi grim slurp wl-clipboard ttf-font-awesome ttf-dejavu
```

### Optional for SSH

```shell
sudo systemctl enable sshd
```

```shell
sudo systemctl start sshd
```

### Hyprland autostart

```shell
sudo nano ~/.bash_profile
```
```config
if [[ -z "${DISPLAY}" ]] && [[ "${XDG_VTNR}" -eq 1 ]]; then
  Hyprland
fi
```

### Autologin

```shell
sudo nano /etc/systemd/logind.conf
```
```config
  NAutoVTs=1 #NAutoVTs=6 uncomment & edit
  ReserveVT=1 #ReserveVT=6 uncomment & edit
```

```shell
sudo nano /etc/systemd/system/autologin@.service
```
```
[Unit]
Description=Automatic Login
After=systemd-user-sessions.service

[Service]
ExecStart=-/usr/bin/agetty --autologin *your-username* --noclear %I $TERM
Type=idle
Restart=always
RestartSec=0

[Install]
WantedBy=getty.target
```

```shell
sudo systemctl enable autologin@tty1.service
```

### Waybar

```shell
sudo cp /etc/xdg/waybar/ ~/.config/waybar/
```
```
  # Then edit the config files at ~/.config/waybar/ as you like
```
