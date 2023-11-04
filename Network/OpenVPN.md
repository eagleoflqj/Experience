# 安装
## Ubuntu
```sh
apt install openvpn
```
## Windows
```sh
choco install openvpn
```
## macOS
```sh
brew install tunnelblick
```
## Android
[ics-openvpn](https://github.com/schwabe/ics-openvpn) F-Droid下载apk
## iOS
[Passepartout](https://github.com/passepartoutvpn/passepartout-apple)
# 证书和密钥
新建openvpn目录
```sh
# CA
openssl genrsa -out ca.key
openssl req -new -x509 -key ca.key -out ca.crt -days 365 -subj /CN=OpenVPN-CA/
# Server
openssl genrsa -out server.key
openssl req -new -key server.key -out server.csr -subj /CN=server/
openssl x509 -req -in server.csr -out server.crt -days 365 -set_serial 0 -CA ca.crt -CAkey ca.key
openssl dhparam -out dh2048.pem 2048
# Client
openssl genrsa -out client.key
openssl req -new -key client.key -out client.csr -subj /CN=用户名/
openssl x509 -req -in client.csr -out client.crt -days 365 -set_serial 0 -CA ca.crt -CAkey ca.key
```
* 每个client的Common Name必须唯一，否则只分配一个IP
* Client和Server的openvpn目录下需要的文件见配置
# 配置
使用 [My OVPN](https://github.com/LibreService/my_ovpn) （客户端所有文件嵌入一个ovpn文件）
或手动如下
## Server
解压`/usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz`到openvpn目录
```sh
# 监听IP
;local a.b.c.d
# 监听端口
port 1194
# 协议
;proto tcp
proto udp
# 模式
;dev tap # 桥接
dev tun # 路由
# SSL/TLS参数
ca ca.crt
cert server.crt
key server.key
# Diffie-Hellman参数
dh dh2048.pem
# 网络拓扑，默认net30以支持v2.0.9及以下Windows客户端
topology subnet
# 路由模式的地址池，桥接模式则注释
server 10.8.0.0 255.255.255.0
# CN和IP的映射文件
ifconfig-pool-persist /var/log/openvpn/ipp.txt
# 修改客户端路由表
push "route 子网 掩码"
# 客户端流量全部经由VPN
push "redirect-gateway def1 bypass-dhcp"
# 允许客户端之间通信
client-to-client
# HMAC Firewall
;tls-auth ta.key 0
```
## Client
### Linux
复制`/usr/share/doc/openvpn[-版本]/examples/sample-config-files/client.conf`到openvpn目录
### Windows
复制`C:\Program Files\OpenVPN\sample-config\client.ovpn`到`~\OpenVPN\config`
### Android
复制配置好的client.ovpn及所需证书、私钥到openvpn目录
```sh
# 声明此配置为client配置
client
# 与server保持一致
dev tun
proto udp
# 指定server
remote 地址 1194
# SSL/TLS参数
ca ca.crt
cert client.crt
key client.key
# EKU验证
;remote-cert-tls server
# HMAC Firewall
;tls-auth ta.key 1
```
# 启动
关闭AppArmor/SELinux
```sh
openvpn server.conf
```
确保IP转发开启
```sh
cat /prod/sys/net/ipv4/ip_forward
```
作为客户端的网关，还应
```sh
iptables -t nat -I POSTROUTING -o 外网网卡 -s 10.8.0.0/24 -j MASQUERADE
```
# 连接
client.ovpn
## Linux
```sh
openvpn client.conf
```
## Windows
* 启动OpenVPN GUI
* 右键任务栏图标，导入配置文件
## Android
* 添加
* 点击配置文件
## iOS
* 将*.ovpn复制到Passepartout目录
* 添加
* 设置DNS为与server相同的DNS，以使用server作为网关
