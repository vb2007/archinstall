# Fixing laggy WinBoat UI on Wayland

Unlike other Electron apps that run fine on Wayland after applying some compatibility config values in `hyprland.conf`, WinBoat's UI is extemely laggy and unresponsive by default. [KNOWN ISSUE](https://github.com/TibixDev/winboat/issues/308)

## Modifying the desktop entry

Copy WinBoat's desktop entry to a local directory:

```shell
sudo cp -r /usr/share/applications/winboat.desktop ~/.local/share/applications/winboat.desktop
```

Open the freshly copied file in a text editor, then modify it's `Exec=` configuration to:

```
Exec=env ELECTRON_OZONE_PLATFORM_HINT=x11 winboat %U
```

Update the desktop database with:

```shell
update-desktop-database ~/.local/share/applications/
```

And make sure your app launcher correctly recognizes the modified desktop entry:

```shell
grep -r "winboat" ~/.local/share/applications/
```

If the modifications were successful, WinBoat should run smoothly after relaunching it.
