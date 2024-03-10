# Archinstall

## Commands right after boot

loadkeys hu
timedatectl set-ntp true

lsblk
cfdisk /dev/sdX # where X is the correct disk
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

mkfs.fat -F32 /dev/sdX1
mkswap /dev/sdX2
swapon /dev/sdX2
mkfs.ext4 /dev/sdX3

mount /dev/sdX3 /mnt
mkdir /mnt/boot
mount /dev/sdX1 /mnt/boot

nano /etc/pacman.conf
```
  #[multilib] # <-- uncomment
  #Include = /etc/pacman.d/mirrorlist # <- uncomment
  #Color # <- uncomment
  #ParallelDownloads = 5 # <- uncomment
  ILoveCandy # add line
```

pacstrap /mnt base base-devel linux linux-firmware git nano [intel or amd]-ucode
genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Europe/Budapest /etc/localtime # or other relevant timezone
hwclock --systohc

nano /etc/locale.gen
```  #en_US.UTF-8 UTF-8 # <- uncomment```

locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf

echo [választott hostname] > /etc/hostname
nano /etc/hosts
```
  127.0.0.1 [TAB] localhost
  ::1 [TAB] localhost
  127.0.0.1 [TAB] [choosen hostname].localdomain [choosen hostname]
```

nano /etc/vconsole.conf
```  #KEYMAP=us # <- change to: KEYMAP=hu```

mkinitpcio -P

pacman -S networkmanager
systemctl enable NetworkManager

passwd
```  [chosen root password here, twice]```

useradd -m [choosen username]
passwd [choosen username]
usermod -aG wheel,storage,power [choosen username]
nano /etc/sudoers
```
  #%wheel ALL=(ALL:ALL) ALL # <- uncomment
  #[optional]# Defaults timestamp_timeout=[when using sudo... 0 = always ask for password, positive number = don't ask for X minutes, negative number = only ask once in a session]
```

pacman -S grub efibootmgr

grub-install --target=x86_64-efi --efi-directory=/boot
grub-mkconfig -o /boot/grub/grub.cfg

exit
reboot

## Dualboot to Windows

c # to enter grub shell
ls # to list disks and partitions
ls (hd0,gpt1)/ # go through partitions, and find the /efi folder
#### then boot to Arch
sudo nano /boot/grub/grub.cfg
```
  # beneath the 'Arch Linux' part
   menuentry 'Winfos 10/11'{
     insmod part_gpt
     insmod chain
     set root=(hd0,gpt1) # or where the /efi folder was
     chainloader /efi/Microsoft/Boot/bootmgfw.efi
     boot
   }
```
