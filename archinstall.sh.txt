#/!bin/bash
sgdisk --xap-all /dev/sda
cgdisk /dev/sda
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
pacstrap -i /mnt base base-devel
genfstab -U -p /mnt >> /mnt/etc/fstab
arch-chroot /mnt /bin/bash
nano /etc/locale.gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8
ln -s /usr/share/zoneinfo/America/Chicago /etc/localtime
hwclock --systohc --utc
echo archlinux > /etc/hostname
nano /etc/hosts
pacman -S wireless_tools wpa_supplicant wpa_actiond dialog
nano /etc/pacman.conf
pacman -Sy
passwd
useradd -m -g users -G wheel,storage,power -s /bin/bash fillips
passwd fillips
pacman -S sudo
EDITOR=nano visudo
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=archlinux --recheck
grub-mkconfig -o /boot/grub/grub.cfg
exit
reboot




