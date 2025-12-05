# Fixing wayland support & audio issues with Plex desktop

After installing Plex desktop from the AUR, several issues occured for me on wayland.

## Preparations

Copy the dekstop entry to your local applications directory, so you don't have to repeat this step everytime Plex updates:

```shell
sudo cp -r /usr/share/applications/tv.plex.PlexDesktop.desktop ~/.local/share/applications/tv.plex.PlexDesktop.desktop
```

When you're done with the desktop entry modification(s) (window and/or audio fix) mentioned below, don't forget to refresh the desktop entries:

```shell
update-desktop-database ~/.local/share/applications/
```

## Fixing invisible/buggy window

Modify the desktop entry to launch Plex correctly:

```shell
sudo nano ~/.local/share/applications/tv.plex.PlexDesktop.desktop
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
