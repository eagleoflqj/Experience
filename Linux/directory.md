## /bin 单用户维护模式下能执行的命令目录
## /boot 开机使用的文件目录
## /boot/grub grub引导装载程序文件目录
## /boot/vmlinuz* Linux内核文件
## /dev 设备文件目录
## /dev/hd* IDE盘
## /dev/log rsyslog的套接字
## /dev/md* 软阵列设备
## /dev/nvme* NVMe盘
## /dev/pts 伪终端目录
## /dev/sd* SATA盘、U盘
## /dev/null 无底洞文件
## /dev/zero 无底洞、无限空字符文件
## /etc 系统配置文件目录
## /etc/fstab 自动挂载配置
```
分区 挂载目录 文件系统 选项 dump选项 fsck选项
```
* 分区为设备目录 或 UUID=分区UUID
* 选项：默认defaults
* dump选项：0不备份，1每天备份，2不定期备份
* fsck选项：0不检验，1最早检验（根目录），2待1级别检验结束后检验
## /etc/group 用户组信息
## /etc/hosts hosts文件
## /etc/init.d 服务默认启动脚本目录
```sh
# Default-Start:      2 3 4 5 # 启动的运行等级
# Default-Stop:       0 1 6 # 停止的运行等级
```
## /etc/network/interfaces 网卡配置
```sh
auto 网卡
iface 网卡 inet static
address IP地址
netmask 掩码
gateway 网关

iface 网卡 inet static
address 其他IP地址/掩码位数

dns-nameservers DNS服务器
```
## /etc/os-release 发行版信息
## /etc/passwd 用户信息
## /etc/rc*.d 各运行等级的服务脚本链接目录
## /etc/rsyslog.conf rsyslog配置
### ubuntu风格
```sh
# unix套接字
module(load="imuxsock")
# udp
module(load="imudp")
input(type="imudp" port="514")
# tcp
module(load="imtcp")
input(type="imtcp" port="514")
# klog
module(load="imklog" permitnonkernelfacility="on") # 允许非内核消息
# 采用传统时间戳
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat # 需要更高精度时间则注释本行
# 过滤重复信息
$RepeatedMsgReduction on
# 日志文件默认权限
$FileOwner syslog # 所有者
$FileGroup adm # 所属组
$FileCreateMode 0640 # 文件权限
$DirCreateMode 0755 # 目录权限
$Umask 0022 # 掩码，与上述权限叠加
# 为监听514端口必须以root启动
$PrivDropToUser syslog # 启动后进程所属用户，名称不存在则不切换
$PrivDropToGroup syslog # 启动后进程所属组，同上
# 工作文件目录
$WorkDirectory /var/spool/rsyslog
# 包含其他配置文件
$IncludeConfig /etc/rsyslog.d/*.conf
# 增加其他监听套接字
$AddUnixListenSocket /var/spool/postfix/dev/log
```
### centos风格
```sh
$ModLoad imudp
$InputUDPServerRun 514
```
### 规则
* selectors 空白符 action，selectors为;隔开的selector，大小写不敏感
* selector：facilities.\[前缀\]priority，facilities为,隔开的facility，取值参见syslog(3)，不含前缀log_
* priority取值none表示该facility的任何级别不适用此行规则

前缀|意义
-|-
无|本级和高级
=|本级
!|低级
!=|非本级

action|意义
-|-
\[-\]日志文件|前缀-表示不对每条消息强制写回磁盘
stop|丢弃消息
* 若有多个action，则第二个action开始每个以&空格为前缀独占一行
* selector：:property, [!]compare-operation, "value"，根据属性匹配消息
## /etc/rsyslog.d rsyslog其他配置目录
## /etc/shadow 用户密码
## /etc/ssl/certs 安装的证书目录
## /etc/systemd systemd配置目录
## /etc/systemd/system/*.target.wants 实现\*目标需要启动的服务目录
包含指向实际服务的软链接
## /etc/udev/rules.d 设备配置规则目录
* .rules规则文件按文件名次序读取，若规则匹配则终止
```sh
# 绑定MAC地址和网卡名
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="MAC地址", NAME="网卡名"
```
## /etc/X11 X Window配置文件目录
## /home/用户名 用户家目录
## /lib 函数库目录
## /lib/modules 内核模块目录
## /lost+found 存放文件系统发生错误时一些丢失的片段
## /media 可删除的设备目录
## /mnt 挂载的设备目录
WSL中Windows盘位于此
## /opt 第三方软件目录
## /proc 状态信息目录（虚拟）
## /proc/cpuinfo CPU信息
### 查看cpu型号及逻辑处理器数
```sh
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
```
## /proc/interrupts 注册的中断
## /proc/kallsyms 内核符号的内存地址
## /proc/modules 加载的内核模块
## /proc/partitions 设备分区信息
## /proc/self 指向`/proc/当前进程号`
## /proc/进程号 进程信息
### 查看输入输出信息
```sh
ls -l fd
```
### 查看启动命令
```sh
cat cmdline
```
## /proc/进程号/exe 指向可执行文件
## /proc/sys/kernel/yama/ptrace_scope 进程调试选项
值|意义
-|-
0|同一用户的进程
1|父进程
2|只允许管理员调试
3|不可调试，设置后必须重启才能取消
## /root 管理员家目录
## /sbin 系统命令目录
## /srv 服务数据目录
## /sys 内核相关信息目录（虚拟）
## /tmp 临时文件目录
## /usr UNIX Software Resource目录
## /usr/bin 用户命令目录
## /usr/include 头文件目录
## /usr/lib 应用的函数库、目标文件目录
## /usr/local 管理员下载的软件目录
## /usr/sbin 其他系统命令目录
## /usr/share 共享文件目录
## /usr/share/ca-certificates/ 机构证书目录
## /usr/share/doc 软件文档目录
## /usr/share/fonts 字体目录
### 添加字体
新建自体名目录（如Consolas），将C:/Windows/fonts中的字体（通常为*.ttf、\*b.ttf、\*i.ttf、\*z.ttf）复制到目录下
## /usr/share/man 手册目录
## /usr/share/zoneinfo 时区信息目录
## /usr/src 源码目录
## /var 变动文件目录
## /var/cache 应用暂存文件目录
## /var/lib 应用数据文件目录
## /var/lock 锁目录
## /var/log 日志目录
## /var/log/btmp 失败登录记录
## /var/log/wtmp 用户登录记录
## /var/run 服务的PID目录
## /var/spool 队列数据目录
## /var/spool/cron 工作排程数据目录