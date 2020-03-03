# 安装
```sh
apt install tcpdump
```
# 说明
* 默认时间显示格式为 时:分:秒.小数秒
* 程序结束或收到SIGUSR1信号后，tcpdump将输出捕获并处理的包数、过滤器收到的包数、由于缓冲区太小而被内核丢弃的包数
# 命令
```sh
tcpdump [选项] [pcap-filter表达式]
```
选项|意义
-|-
-#|行首输出包编号
-A|ASCII输出除链路头外的内容
-D|列出可用网卡
-L|查看接口支持的链路类型
-Q in\|out|指定捕获的包的方向，默认inout
-X|hex和ASCII输出除链路头外的内容
-XX|hex和ASCII输出内容
-Z 用户|以root运行时，指定打开文件的用户
-c 整数|最多捕获包数
-e|输出链路头
-i 网卡|指定网卡，默认-D列出的第一个（lo除外）
-n|不将地址或端口转换成名称
-q|输出较少的协议信息
-r 文件|指定二进制输入文件
-t|不输出时间
-tt|时间为UNIX时间戳
-ttt|时间为和上一条的时间差
-tttt|时间前加日期
-ttttt|时间为和第一条的时间差
-v\|-vv\|-vvv|显示详情
-x|hex输出除链路头外的内容
-xx|hex输出内容
-w 文件|指定二进制输出文件
* lo只有方向in
# pcap-filter
协议 方向 类型 值
## 协议
ether
arp
ip
ip6
icmp
icmp6
tcp
udp
## 方向
* src
* dst
* src or dst（默认）
* src and dst
## 类型
类型和值|意义
-|-
host 地址|单一地址，由协议决定格式，默认
net 子网\[/掩码位数\| mask 掩码\]|子网
port 端口|单一端口
portrange 端口下限-端口上限|端口范围
## 其他
表达式|意义
-|-
broadcast|以太网广播
ip broadcast|IP广播
greater 字节数|长度下限（含端点，含链路头，下同）
less 字节数|长度上限
