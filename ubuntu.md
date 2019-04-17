# 基本操作
## 打开终端
Ctrl+Alt+T
## 切换登录终端
Ctrl+Alt+F1至F7
## 打开当前文件夹
~.bashrc
```sh
alias oo='nautilus . &'
```
## 安装包
```sh
apt install 包名
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
## 解决vim异常
```sh
apt remove vim-tiny
apt install vim
```
# 使用rtl8821cu
## 安装驱动
```sh
git clone https://github.com/whitebatman2/rtl8821CU
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