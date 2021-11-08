---
title: "Installing Arch Linux from scratch"
date: "2015-02-21"
tags: ["linux"]
---
This is a dump of commands I used, for future reference. Ideallym this should be an Ansible playbook or something to make it automated.

There's more than one way to skin a cat, of course, but here's my way:
* using WiFi throughout the install
* a system with an existing Linux install
* I prefer zsh over bash
* I also prefer GNOME3 for its ease of use, although i3 is pretty nice too

You can read more about [my setup](/blog/my-setup/) and tools I use.

```bash 
$ wifi-menu
$ fdisk -l
$ mkfs.ext4 /dev/sdxY
$ mkswap /dev/sdxY
$ swapon /dev/sdxY
$ mount /dev/sdxR /mnt
$ mkdir -p /mnt/home
$ nano /etc/pacman.d/mirrorlist
$ pacstrap -i /mnt base base-devel
$ genfstab -U -p /mnt >> /mnt/etc/fstab
$ arch-chroot /mnt /bin/bash
$ pacman -S zsh vim
$ nano /etc/locale.gen
$ locale-gen
$ echo LANG=en_US.UTF-8 > /etc/locale.conf
$ export LANG=en_US.UTF-8
$ ln -s /usr/share/zoneinfo/Asia/Indian /etc/localtime
$ hwclock --systohc --utc
$ echo archlinux > /etc/hostname
$ vim /etc/pacman.conf # uncomment multilib repo
$ pacman -Syu
$ passwd # set root password
$ useradd -m -g users -G wheel,storage,power -s /bin/zsh sumesh #
$ passwd sumesh
$ pacman -S sudo
$ EDITOR=nano visudo #uncomment %wheel
$ pacman -S grub
$ grub-install --target=i386-pc --recheck /dev/sda
$ pacman -S os-prober
$ grub-mkconfig -o /boot/grub/grub.cfg
$ pacman -S iw wpa_supplicant wpa_actiond
$ pacman -S xorg-server xorg-server-utils xorg-xinit mesa xf86-video-intel lib32-intel-dri
$ pacman -S xorg-twm xorg-xclock xterm
$ startx # to test drivers
$ pacman -S gnome gdm
$ pacman -S xf86-input-synaptics xf86-video-intel alsa-utils pulseaudio
$ echo "exec gnome-shell" > ~/.xinitrc
$ echo "[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch" >> /etc/pacman.conf
$ pacman -Sy yaourt # install packages from AUR
$ pacman -S vlc terminator guake git mysql apache nginx tmux transmission-gtk gnome-tweak-tool ncdu htop coreutils wget curl postgresql dropbox skype rdesktop xchat gimp zsh quodlibet
$ yaourt ttf-ms-fonts codecs64 
$ exit
$ reboot

$ yaourt -Syu # Update AUR packages
$ sudo pacman -Sy # sync repos
$ sudo pacman -Syu # update system
$ sudo pacman -R # remove package
$ sudo pacman -Q # check for installed package
```