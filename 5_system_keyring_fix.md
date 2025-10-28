# Fixing compatibility issues with the system keyring

Many applications use the system keyring for storing access keys and other types of  auth tokes.

I use KeePassXC for storing & managing them, but some applications (like Element desktop) fail to save their auth tokens.

## Installing & configuring KeePassXC

First, intall KeePassXC:

```shell
sudo pacman -S KeePassXC
```

Then create a database in its GUI, and set the following settings:

### Application settings

- General
  - Startup
    - [x] Start only a single instance of KeePassXC
    - [x] Automatically launch KeePassXC at system startup
    - [x] Minimize window at application startup
    - [x] Minimize window after unlocking database
  - User interface
    - [x] Show toolbar
    - [x] Show menubar
    - [ ] Show passwords in color
    - [ ] Use monospaced font for notes
    - [x] Minimize instead of app exit
    - [x] Show a system tray icon
- Secret Service Integration
  - [x] Enable KeepassXC Freedesktop.org Secret Service integration
  - [ ] Show notification when passwords are retrieved by clients
  - [ ] Confirm when passwords are retrieved by clients
  - [ ] Confirm when clients request entry deletion
  - [ ] Prompt to unlock database before searching

### Database settings

- Secret Service Integration
  - [x] Expose entries under this group: **Root**

## Getting D-Bus to work correctly

Install **libsecret**:

```shell
sudo pacman -S libsecret
```

Creater the D-Bus Service Symlink

```shell
sudo ln -s /usr/share/dbus-1/services/org.keepassxc.KeePassXC.service /usr/share/dbus-1/services/org.freedesktop.secrets.service
```

Verify that the symlink is correctly set:

```shell
ls -l /usr/share/dbus-1/services/org.freedesktop.secrets.service
```

*A correct output looks something like this:*

```shell
lrwxrwxrwx 1 root root 58 Oct 28 21:36 /usr/share/dbus-1/services/org.freedesktop.secrets.service -> /usr/share/dbus-1/services/org.keepassxc.KeePassXC.service
```

( *Element-specific* ) Start Element once from the terminal with a password-store option:

```shell
element-desktop --password-store="gnome-libsecret"
```
