# Installing yay AUR helper

Installing required packages

```shell
sudo pacman -Syu
```

```shell
sudo pacman -S --needed base-devel git
```

Cloning yay

```shell
git clone https://aur.archlinux.org/yay.git
```

```shell
cd yay
```

Building it

```shell
makepkg -si
```

Verifying installation

```shell
yay --version
```
