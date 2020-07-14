# 网络
## arp 操作ARP缓存
```sh
arp [选项] [模式]
```
选项|意义
-|-
-v|详细输出
-i 网卡|指定网卡
-t 类型|硬件类型，如ether

模式|意义
-|-
\[主机\]|查看缓存表
-d 主机|删除表项
-s 主机 MAC地址|添加表项
## curl 发送请求
```sh
curl [参数] URL
```
参数|意义
-|-
-d 字符串|指定body
-H 请求头|指定一行header
-X 方法|指定方法
--cacert 证书|指定CA证书
--data-binary @文件|指定body
## dhclient DHCP客户端
```sh
dhclient [网卡]
```
## dig DNS查询
```sh
dig [参数] [域名] [类型]
```
参数|意义
-|-
@服务器|指定dns服务器，否则/etc/resolv.conf中的地址，若无可用地址则尝试本地
-p 端口|指定端口，默认53
-4|只采用IPv4服务器
-6|只采用IPv6服务器
* 域名不存在则查询root
* 类型默认A
## etherwake 局域网唤醒
```sh
etherwake [选项] MAC地址
```
选项|意义
-|-
-b|以太网广播
-i 网卡|指定网卡，默认eth0
## ethtool 以太网卡工具
### 查看设置
```sh
ethtool 网卡
```
### 修改设置
```sh
ethtool -s 网卡 参数
```
参数|意义
-|-
wol 唤醒选项|设置Wake-on-Lan

唤醒选项|意义
-|-
g|魔包唤醒
d|禁用
## ifconfig 输出网络配置
```sh
ifconfig
```
## ip 网络配置
### 查看网卡及ip
```sh
ip address
```
### 添加IP
```sh
ip address add dev 网卡 local IP地址/掩码长度 peer IP地址/掩码长度
```
### 删除IP
```sh
ip address del IP地址[/掩码长度] dev 网卡
```
### 启停网卡
```sh
ip link set 网卡 up|down
```
### 查看邻居
```sh
ip neigh
```
## iperf3 测网速
通用选项|意义
-|-
-p 端口|指定端口，默认5201
### 服务器
```sh
iperf3 -s [选项]
```
选项|意义
-|-
-1|接受一个客户端连接后退出
### 客户端
```sh
iperf3 -c 主机 [选项]
```
选项|意义
-|-
-b 数字\[kmg\]|目标速率，0为无限；UDP默认1m，TCP默认0
-u|UDP，默认TCP
-R|下载，默认上传
## iwlist 查看无线网络
### 扫描无线网
```sh
iwlist [网卡] scan
```
## nc 传输层工具
```sh
nc [选项] 主机 端口
```
选项|意义
-|-
-4|只使用IPv4
-6|只使用IPv6
-l|服务端监听，默认客户端
-p 端口|指定客户端端口
-u|UDP，默认TCP
-v|输出详细信息
-w 秒数|客户端超时时间，默认无
-z|扫描端口
## nmcli NetworkManager客户端
### 连接wifi
```sh
nmcli device wifi connect 接入点 password 密码
```
### 查看连接
```sh
nmcli connection show
```
### 断开网络
```sh
nmcli connection down 连接UUID
```
### 重连网络
```sh
nmcli connection up 连接UUID
```
## ping ICMP测通
```sh
ping [选项] 主机
```
选项|意义
-|-
-a|ping的同时响铃（需要终端支持）
-c 次数|ping给定次，默认无限次
-i 秒数|指定请求间隔，默认1秒
-w 秒数|几秒后结束程序
## ss socket状态
```sh
ss [选项] [state 状态] [表达式]
```
选项|意义
-|-
-4|显示仅监听IPv4
-6|显示监听IPv6
-a|显示监听和连接
-i|显示TCP信息
-l|只显示监听，默认只显示连接
-n|端口显示为端口号，默认监听该端口常用应用名
-o|显示定时器
-p|显示进程
-r|主机名显示为域名，默认IP
-s|显示统计信息
-t|显示TCP
-u|显示UDP
-x|显示Unix域
-H|不显示标题
* 标准TCP状态：established、syn-sent、syn-recv、fin-wait-1、fin-wait-2、time-wait、closed、close-wait、last-ack、listening、closing

组合状态|意义
-|-
all|所有状态
connected|除listening和closed
synchronized|除syn-sent

表达式|意义
-|-
src \[IP\[/掩码位数\]\]\[:端口\]|本地位置
dst \[IP\[/掩码位数\]\]\[:端口\]|对端位置
## ssh 安全shell
```sh
ssh [选项] 目标 [命令]
```
选项|意义
-|-
-i 文件|指定使用的客户端配置
-p 端口|指定端口
-X|传送X11，需要DISPLAY=localhost:0
-x|不传送X11

客户端全局配置文件/etc/ssh/ssh_config，定义了默认的私钥（包括~/.ssh/id_rsa）  
客户端用户配置文件~/.ssh/config  
服务器配置文件/etc/ssh/sshd_config
### 客户端用户配置
```python
Host 别名
  HostName IP地址或域名 # 若无HostName，则Host应为全名
  Port 端口，默认22
  User 默认用户名
  IdentityFile ~/.ssh/私钥 # 免密登录
```
配置免密登录时将公钥上传到服务器  
登录服务器，追加公钥到~/.ssh/authorized_keys  
确保.ssh权限为700，authorized_keys权限为600
```sh
ssh [用户名@]别名
```
### 服务器配置
```python
ListenAddress IP地址 # 监听地址
AllowGroups root sudo # 允许登录的组
PermitRootLogin no # 禁止root登录
PasswordAuthentication no # 禁止密码登录
X11Forwarding yes # 传送X11信息
ClientAliveInterval 60 # 保持连接
AuthorizedKeysFile .ssh/authorized_keys # 公钥存放位置，以家目录为工作目录
```
## ssh-keygen 生成公私钥对
```sh
ssh-keygen -t rsa [-f 私钥名] [-C 注释]
```
若未指定私钥名，则交互式指定
## update-ca-certificates 更新证书列表
* 将证书置于`/usr/share/ca-certificates`
* 在`/etc/ca-certificates.conf`中以`/usr/share/ca-certificates`为工作目录添加或删除证书
```sh
update-ca-certificates
```
* 上述命令修改`/etc/ssl/certs`下的软链接
## wget 下载文件
```sh
wget [选项] [URL]
```
选项|意义
-|-
-c|继续之前的下载
-i 文件|从文件按行读取URL
-P 目录|下载文件存放目录，默认`.`

* `-c`比较URL末端和当前目录下的同名文件，视文件为下载的前一部分，并从文件大小的字节开始请求服务器
