# 配置
## /etc/default/grub
```sh
GRUB_DEFAULT=0 # 超时则选择菜单第一项
GRUB_TIMEOUT_STYLE=menu # hidden表示不显示菜单，countdown表示只显示倒计时
GRUB_TIMEOUT=5 # 超时时间5秒
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash" # 添加到普通模式的内核参数
GRUB_CMDLINE_LINUX="" # 添加到所有模式的内核参数
#GRUB_TERMINAL=console # 默认gfxterm，即图形界面
#GRUB_GFXMODE=640x480 # 默认auto
```
## /boot/grub/grub.cfg
```sh
set default=0
set timeout_sytle=menu
set timeout=5
menuentry "标题" {
    insmod ext2 # 加载ext2.mod
    set root='hd0,1' # 内核文件在第一块硬盘，第一个分区
    linux /boot/vmlinuz root=/dev/sda1 ro # 指定内核（(hd0,1)的路径）及其参数
    initrd /boot/initramfs # 指定内存文件系统
}
```
* 一般由grub-mkconfig生成
# 命令
## grub-install 安装grub到硬盘
```sh
grub-install 硬盘
```
## grub-mkconfig 生成实际配置
```sh
grub-mkconfig [-o 文件]
```
* 默认写入标准输出
* 读取`/etc/default/grub`、`/etc/default/grub.d/*`，检查`/boot`下镜像
## update-grub 更新实际配置
```sh
update-grub # 等价于grub-mkconfig -o /boot/grub/grub.cfg
```
