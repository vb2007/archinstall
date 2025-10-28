# Fixing wayland support & audio issues with Plex desktop

After installing Plex desktop from the AUR, several issues occured for me on wayland.

## Fixing invisible/buggy window

Modify the desktop entry to launch Plex correctly:

```shell
sudo nano /usr/share/applications/tv.plex.PlexDesktop.desktop
```

Change launch options at the `EXEC=` line:

```shell
Exec=env QT_QPA_PLATFORM=xcb Plex
```

## Fixing audio issues (muted/low quality audio)

Simply installing the `pipewire-pulse` compatibility layer package should resolve every audio issues:

```shell
sudo pacman -S pipewire-pulse
```
