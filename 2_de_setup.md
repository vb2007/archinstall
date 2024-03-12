# Installing a desktop environment (and other necessary things)

sudo pacman -S hyprland waybar neofetch kitty thunar libreoffice openssh

sudo systemctl enable sshd

sudo systemctl start sshd

### Hyprland autostart

sudo nano ~/.bash_profile
```
  if [[ -z "${DISPLAY}" ]] && [[ "${XDG_VTNR}" -eq 1 ]]; then
    Hyprland
  fi
```

### Autologin

sudo nano /etc/systemd/logind.conf
```
  NAutoVTs=1 #NAutoVTs=6 uncomment & edit
  ReserveVT=1 #ReserveVT=6 uncomment & edit
```

sudo nano /etc/systemd/system/autologin@.service
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

sudo systemctl enable autologin@tty1.service

### Waybar
sudo cp /etc/xdg/waybar/ ~/.config/waybar/
```
  # Then edit the config files at ~/.config/waybar/ as you like
```