# 安装
需要联网
## 启动盘
Boot Arch Linux (x86_64)
## 分区
以/dev/sda为例
```sh
cfdisk
```
选择dos  
依次选择New、primary、Bootable、Write、Quit
## 格式化
```sh
mkfs.ext4 /dev/sda1
```
## 挂载
```sh
mount /dev/sda1 /mnt
```
## 安装Arch Linux基础系统
```sh
pacstrap /mnt base base-devel linux
```
## 自动挂载
```sh
genfstab -U /mnt >> /mnt/etc/fstab
```
## 切换根目录
```sh
arch-chroot /mnt /bin/bash
```
## 设置root密码
```sh
passwd
```
## 安装必要工具
```sh
pacman -Syu dhcp grub vim
```
## 设置本地化
/etc/locale.gen
```
en_US.UTF-8 UTF-8
```
```sh
locale-gen
```
/etc/locale.conf
```
LANG=en_US.UTF-8
```
## 设置时区
```sh
ln -s /usr/share/zoneinfo/洲/城市 /etc/localtime
```
## 设置时间
```sh
hwclock --systohc --utc
```
## 设置主机名
/etc/hostname
## 安装grub
```sh
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```
## 结束
```sh
exit
reboot
```
