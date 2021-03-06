# Arch Install

## Desktop Install
Making sure we're connected to the internet.
```
$ ping 1.1.1.1
```
update the system clock.
```
$ timedatectl set-ntp true
```
## Partitioning
I think cfdisk is a better experience compared to fdisk
```
$ lsblk
$ cfdisk /dev/${drive}
```
for a normal install I prefer a single partition for my data along with an efi and swap.
```
$ mkfs.fat -F32 /dev/sdX1
$ mkfs.ext4 /dev/sdX3
$ mkswap /dev/sdX2
$ swapon /dev/sdX2
```
## Minimal Install
```
$ mount /dev/sdX3 /mnt
$ mkdir /mnt/boot/
$ mkdir /mnt/boot/efi
$ mount /dev/sdX1 /mnt/boot/efi
```
Probably don't need linux-firmware but I always install it
```
$ pacstrap /mnt base linux linux-firmware
$ genfstab -U /mnt >> /mnt/etc/fstab
$ arch-chroot /mnt
```
general region config
```
$ ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime
$ hwclock --systohc --utc
$ locale-gen
$ echo 'LANG=en_US.UTF-8' > /etc/locale.conf
```
change this to whatever you want your hostname to be
```
$ echo Archlinux > /etc/hostname
```
add root password
```
passwd
```
make sure networking works on reboot
```
$ pacman -S grub efibootmgr networkmanager sudo
$ systemctl enable NetworkManager
```
when installing on uefi system
```
$ grub-install --target=x86_64-efi --efi-directory=/boot/efi
$ grub-mkconfig -o /boot/grub/grub.cfg
```
## New User
```
$ useradd -G wheel -m ${user}
```
## Post-Install
packages (there are a number of unnessesary packages here, but I find it cleaner to install them all)
```
$ pacman -S git vim firefox
$ pacman -S xorg-server xorg-xinit xorg-xrandr
$ pacman -S i3
```
nerd fonts is the easiest way to manage fonts
```
$ cd /var/tmp
$ git clone https://github.com/ryanoasis/nerd-fonts.git --depth 1
$ cd nerd-fonts && ./install.sh
```
xorg i3 setup
```
$ echo "exec i3" >> ~/.xinitrc
```

