# apt
## 列出包
```sh
apt list [选项]
```
选项|意义
-|-
--installed|已安装
--upgradable|可更新
## 更新目录
```sh
apt update
```
## 更新
```sh
apt upgrade [包]
```
## 搜索
```sh
apt search 包
```
## 安装
```sh
apt install 包
```
## 查看文件所属包
```sh
apt-file search 文件
```
# GNOME
## 打开终端
Ctrl+Alt+T
* 非登录bash
## 切换登录终端
Ctrl+Alt+F1至F7
## 打开当前文件夹
~/.bashrc
```sh
alias oo='nautilus . &'
```
## 编辑器
```sh
gedit 文件
```
## 截图
保存位置：~/图片

快捷键|效果
-|-
PrintScreen|全部有信号的屏幕，按分辨率比例和相对位置
alt+PrintScreen|当前活动窗口
Shift+PrintScreen|鼠标框选
# 异常
## dash
上箭头显示^[[A，需将登录shell改为/bin/bash
## vim
```sh
apt remove vim-tiny
apt install vim
```
## 远程登录多次键入密码
/etc/polkit-1/localauthority/50-local.d/45-allow-colord.pkla
```
[Allow Colord all Users]
Identity=unix-user:*
Action=org.freedesktop.color-manager.create-device;org.freedesktop.color-manager.create-profile;org.freedesktop.color-manager.delete-device;org.freedesktop.color-manager.delete-profile;org.freedesktop.color-manager.modify-device;org.freedesktop.color-manager.modify-profile
ResultAny=no
ResultInactive=no
ResultActive=yes

[Allow Package Management all Users]
Identity=unix-user:*
Action=org.debian.apt.*;io.snapcraft.*;org.freedesktop.packagekit.*;com.ubuntu.update-notifier.*
ResultAny=no
ResultInactive=no
ResultActive=yes
```
# 摄像头
```sh
apt install cheese
```
# snap
## 授权
```sh
snap connect 程序:权限
```
权限|
-
removable-media
# 管理驱动
### 查看当前系统的驱动包
```sh
ubuntu-drivers list
```
### 查看需要驱动的设备及其驱动包
```sh
ubuntu-driver devices
```
# 使用rtl8821cu
## 安装驱动
```sh
git clone https://github.com/brektrou/rtl8821CU
cd rtl8821CU
```
按照README.md的指示编译安装
## 识别设备
查看被识别的种类
```sh
lsusb
```
VendorId：0bda  
ProductId：1a2b  
修改/lib/udev/rules.d/40-usb_modeswitch.rules，在SUBSYSTEM行的下面添加
```
# Realtek rtl8821cu
ATTR{idVendor}=="0bda", ATTR{idProduct}="1a2b", RUN+="usb_modeswitch '/%k'"
```
新建/etc/usb_modeswitch.d/0bda:1a2b指明转换目标  
VendorId不变，ProductId参照Windows设备管理器给出的硬件Id
```
# Realtek rtl8821cu
TargetVendor=0x0bda
TargetProduct=0xc820
StandardEject=1
```
重启（否则只能识别其一）  
参考 https://blog.csdn.net/bingo1991/article/details/80633000
# 使用Mellanox IB网卡
## 安装驱动
http://www.mellanox.com/page/software_overview_ib 选择合适版本驱动下载
```sh
tar xzvf MLNX_OFED_LINUX.tar
cd MLNX_OFED_LINUX
./mlnxofedinstall --skip-distro-check --force
reboot
```
## 运行模式
```sh
connectx_port_config [参数]
```
参数|意义
-|-
无|修改
-s|查看
## 状态
```sh
ibstat
ibstatus
```
## 配置
/etc/udev/rules.d/90-ib.rules
```sh
# 绑定网卡名
DRIVERS=="*mlx*", SUBSYSTEM=="net", ACTION=="add",RUN+="/bin/systemctl --no-block start mlnx_interface_mgr@$env{INTERFACE}.service", NAME="ib0"
```
/etc/network/interfaces
```conf
auto ib0
iface ib0 inet static
address IP地址/掩码位数
```
重启
## 启动ib
```sh
systemctl start openibd
systemctl start opensmd
```
### 设置自启
/etc/init.d/opensmd
```sh
# Default-Start: 2 3 4 5
```
```sh
update-rc.d opensmd remove
update-rc.d opensmd defaults
```
