# Archinstall

## Commands right after boot

```shell
loadkeys hu
```

```shell
timedatectl set-ntp true
```

```shell
lsblk
```

Here, X is the disk you want to choose for installation. Could be nvme0nX if you're installing to an NVME SSD:

```shell
cfdisk /dev/sdX #Remember to choose correct disk.
```

Create the following partitions (delete existing ones if needed):
```
New
1G
Type
EFI System
New
[Your machine's total RAM]G
Type
Linux swap
New
[Rest of the space]
Type
Linux filesystem
Write
yes
Quit
```

```shell
mkfs.fat -F32 /dev/sdX1
```

```shell
mkswap /dev/sdX2
```

```shell
swapon /dev/sdX2
```

```shell
mkfs.ext4 /dev/sdX3
```

```shell
mount /dev/sdX3 /mnt
```

```shell
mkdir /mnt/boot
```

```shell
mount /dev/sdX1 /mnt/boot
```

```shell
pacstrap /mnt base base-devel linux linux-firmware git nano [intel or amd]-ucode
```

```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

```shell
arch-chroot /mnt
```

```shell
ln -sf /usr/share/zoneinfo/Europe/Budapest /etc/localtime # or other relevant timezone
```

```shell
hwclock --systohc
```

```shell
nano /etc/pacman.conf
```

Make the following edits to the file:

```
#[multilib] # <-- uncomment
#Include = /etc/pacman.d/mirrorlist # <- uncomment
#ParallelDownloads = 5 # <- uncomment
```

```shell
nano /etc/locale.gen
```

Make the following edits to the file:

```
#en_US.UTF-8 UTF-8 # <- uncomment
```

```shell
locale-gen
```

```shell
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

```shell
echo [chosen hostname] > /etc/hostname
```

```shell
nano /etc/hosts
```

Add the following lines:

```
127.0.0.1 [TAB] localhost
::1 [TAB] localhost
127.0.0.1 [TAB] [chosen hostname].localdomain [chosen hostname]
```

```shell
nano /etc/vconsole.conf
```

Add this line: 

```
KEYMAP=hu
```

```shell
mkinitcpio -P
```

```shell
pacman -Sy
```

```shell
pacman -S networkmanager
```

```shell
systemctl enable NetworkManager
```

```shell
passwd #[chosen root password here, twice]
```

```shell
useradd -m  [chosen username]
```

```shell
passwd [chosen username] #then [chosen password for that username, twice]
```

```shell
usermod -aG wheel,storage,power [chosen username]
```

```shell
nano /etc/sudoers
```

Make the following edits to the file:

```
  #%wheel ALL=(ALL:ALL) ALL # <- uncomment
  #Defaults timestamp_timeout=[when using sudo... 0 = always ask for password, positive number = don't ask for X minutes, negative number = only ask once in a session]

  #%wheel ALL=(ALL:ALL) ALL NOPASSWD: ALL # <- or just uncomment this, so you can sudo without entering the password
```

```shell
pacman -S grub efibootmgr
```

```shell
grub-install --target=x86_64-efi --efi-directory=/boot
```

```shell
grub-mkconfig -o /boot/grub/grub.cfg
```

```shell
exit
```

```shell
reboot
```

## Dualboot to Windows

Enter the grub shell:

```shell
c
```

List disks and partitions:

```shell
ls
```

Go through all your partitions, and find the ```/efi``` folder.

```shell
ls (hd0,gpt1)/
```

Make a note of that disk & partition.

#### Then boot to Arch

```shell
sudo nano /boot/grub/grub.cfg
```

Add these lines beneath (or above, personal preference) the 'Arch Linux' part.

REMEMBER TO USE THE CORRECT DISK & PARTITION COMBINATION, DON'T JUST COPY AND PASTE.

```conf
menuentry 'Windows 10/11'{
  insmod part_gpt
  insmod chain
  set root=(hd0,gpt1) # or where the /efi folder was
  chainloader /efi/Microsoft/Boot/bootmgfw.efi
  boot
}
```
