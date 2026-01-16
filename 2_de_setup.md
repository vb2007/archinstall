# Installing a desktop environment (and other necessary things)

```shell
sudo pacman -S hyprland hyprpaper hyprlock xdg-desktop-portal-hyprland waybar fastfetch kitty cosmic-files cosmic-settings-daemon unzip zip tar p7zip gvfs libreoffice openssh wofi grim slurp wl-clipboard wget curl pipewire pipewire-jack pipewire-pulse wireplumber librewolf ttf-font-awesome ttf-dejavu otf-font-awesome
```

## Nvidia & Intel iGPU drivers

Installing drivers for Nvidia & Intel iGPU

```shell
sudo pacman -S nvidia nvidia-utils egl-wayland
```

Enable modeset for both GPUs (as stated in the [official docs](https://wiki.hypr.land/Nvidia/))

```shell
sudo nano /etc/modprobe.d/nvidia.conf
```

```shell
#Add this line
options nvidia_drm modeset=1
```

Enable early KMS

```shell
sudo nano /etc/mkinitcpio.conf
```

```shell
#Modify this line
MODULES=(i915 nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

Rebuild initramfs

```shell
sudo mkinitcpio -P
```

Add these enviroment variables to hyprland.conf

```shell
sudo nano ~/.config/hypr/hyprland.conf
```

```shell
env = LIBVA_DRIVER_NAME,nvidia
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
```

> [!WARNING]
> Reboot after installing the drivers & making the other modifications

## Optional for SSH

```shell
sudo systemctl enable sshd
```

```shell
sudo systemctl start sshd
```

## Hyprland autostart

```shell
sudo nano ~/.bash_profile
```
```config
if [[ -z "${DISPLAY}" ]] && [[ "${XDG_VTNR}" -eq 1 ]]; then
  Hyprland
fi
```

## Autologin

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

## Waybar

```shell
sudo cp /etc/xdg/waybar/ ~/.config/waybar/
```
```
  # Then edit the config files at ~/.config/waybar/ as you like
```

## Setting a default browser

If you install multiple browser (chromium, librewolf, firefox, so on...), this should be useful.

Getting the current default browser:

```shell
xdg-settings get default-web-browser
```

Setting a default browser (e.g. LibreWolf):

```shell
xdg-settings set default-web-browser librewolf.desktop
```
