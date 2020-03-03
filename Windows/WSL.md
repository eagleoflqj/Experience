# GUI
## 安装
```sh
apt install xorg
apt install xfce4
apt install xrdp
```
## 配置
```sh
sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini
echo xfce4-session >~/.xsession
```
## 启动
```sh
service xrdp start
```
## 连接
Windows远程桌面，localhost:3309
# 问题
## 无法使用lsusb
## nice默认-10
## 无法使用reredirect
