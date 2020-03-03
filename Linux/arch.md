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
pacman -Syu dhcp grub man-db vim
ln -s vim /usr/bin/vi
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
# pacman
## 配置
/etc/pacman.conf
```
Color # 彩色输出
```
## 最快镜像
```sh
pacman -Syu reflector
reflector --verbose --latest 200 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```
## 搜索
```sh
pacman -Ss 包
```
## 安装/更新
```sh
pacman -Syu [包]
```
## 卸载
```sh
pacman -R[su] 包
```
* -su表示一并卸载孤立包
## 查看孤立包
```sh
pacman -Qdt
```
## 卸载孤立包
```sh
pacman -Rs $(pacman -Qdtq)
```
## 查看已安装包
```sh
pacman -Q[s 包]
```
## 查看文件所属包
```sh
pacman -Qo 文件
```
# GNOME
## 安装
```sh
pacman -Syu gnome
systemctl enable gdm
systemctl start gdm
```
## 终端快捷键
Settings -> Devices -> Keyboard Shortcuts -> +
* Name: Launch Terminal
* Command: gnome-terminal
* Shortcut: Ctrl+Alt+T
