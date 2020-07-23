* [Filesystems Hierarchy Standard](https://refspecs.linuxfoundation.org/fhs.shtml)
* 文件名不超过255字符
* 完整路径不超过4096字符
# bin 单用户维护模式下能执行的命令目录
* 必须位于根分区
# boot 开机使用的文件目录
## config 当前内核编译配置
## initrd或initramfs 内存文件系统
```sh
cpio -t <initrd # 列出头部cpio及所占BLOCKS
# cpio -i <initrd # 解开头部
dd if=initrd of=OUTPUT bs=512 skip=BLOCKS
file OUTPUT # 根据类型，重复上述步骤或解压缩
mv OUTPUT{,.?}
gzip/lz4 -d OUTPUT.?
cpio -i <OUTPUT
```
* 当内核不内置读取根分区的驱动时必须使用
## grub grub引导装载程序文件目录
### fonts 字体目录
### grub.cfg grub配置
### grubenv 储存开机时生成的环境变量，1024B
### i386-pc 架构相关模块目录
## System.map 内核符号内存地址
## vmlinuz* Linux内核文件
# dev 设备文件目录
## hd* IDE盘
## kmsg 内核printk缓冲区
## log rsyslog的套接字
## loop* 模拟块设备
## md? 软RAID设备
## null 无底洞文件
## nvme? NVMe盘
## pts 伪终端目录
## sd? SATA盘、U盘
## sd?? 分区
## ttyUSB? USB-UART设备
## zero 无底洞、无限空字符文件
## 卷组 LVM卷组目录
## 卷组/逻辑卷 LVM逻辑卷
# etc 系统配置文件目录
* 必须位于根分区
## adjtime 硬件时钟配置
## anacrontab anacron配置
```
周期天数 延迟分钟 任务 命令
```
## at.allow 允许使用at的用户
* 存在时忽略`/etc/at.deny`
## at.deny 禁止使用at的用户
## cron.allow 允许使用crontab的用户
* 存在时忽略`/etc/cron.deny`
## cron.deny 禁止使用crontab的用户
## crontab cron配置
```
* * * * * 用户名 命令
```
## cups CUPS配置目录
## default 默认配置目录
### grub grub默认配置
### useradd useradd默认配置
```sh
GROUP=999
HOME=/home
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
```
## filesystems 文件系统类型
* 无法推测文件系统时，依次尝试
## fstab 自动挂载配置
```
分区 挂载点 文件系统 挂载选项 dump选项 fsck选项
```
* 分区为`/dev`下路径、`LABEL=卷标`或`UUID=分区UUID`
* dump选项：0不备份，1每天备份，2不定期备份
* fsck选项：0不检验，1最早检验（根目录），2待1级别检验结束后检验
* 挂载选项同`mount`
## group 用户组信息
字段|意义
-|-
1|组名
2|x，原先密码列
3|GID
4|`,`分隔组员用户名，可不含以其为主组的成员

GID|名称
-|-
0|root
1|bin
## gshadow 组密码
字段|意义
-|-
1|组名
2|加密的密码
3|`,`分隔管理员用户名
4|组员用户名，同`/etc/group`
## hostname 主机名
* 由systemd-hostnamed自动生成
## hosts hosts文件
## init.d SysV服务脚本目录
```sh
# Default-Start:      2 3 4 5 # 启动的运行等级
# Default-Stop:       0 1 6 # 停止的运行等级
```
## inputrc Readline库配置
## issue TTY登录前显示的信息
## issue.net telnet登录前显示的信息
符号|意义
-|-
\l|当前tty
\m|CPU架构
\r|内核版本
\S|`/etc/os-release`中的`PRETTY_NAME`
## ld.so.cache 动态链接库列表
* 由ldconfig根据ld.so.conf编译生成
## ld.so.conf 动态链接库目录列表
## locale.conf 本地化设置
## localtime 指向`/usr/share/zoneinfo/当前时区`
## login.defs shadow套件配置
```sh
# 邮箱目录
MAIL_DIR /var/mail

# 默认PATH
ENV_SUPATH PATH=/sbin:/bin:/usr/sbin:/usr/bin
ENV_PATH PATH=/bin:/usr/bin

# 未设置HOME_MODE时家目录umask
UMASK 022

# 密码控制
PASS_MAX_DAYS 99999 # 密码设置后多少天必须更改
PASS_MIN_DAYS # 密码设置后多少天不可更改
PASS_MIN_LEN # 密码长度最小值
PASS_WARN_AGE # 密码到期前多少天警告用户

# 自动生成的UID范围
UID_MIN 1000
UID_MAX 60000
# 系统账户
SYS_UID_MIN 101
SYS_UID_MAX 999

# 自动生成的GID范围
GID_MIN 1000
GID_MAX 60000
# 系统账户
SYS_GID_MIN 101
SYS_GID_MAX 999

# 用户可以使用的（shadow-utils包中）chfn选项
CHFN_RESTRICT rwh

# 密码加密方法
ENCRYPT_METHOD SHA512

# UID=GID、用户名=主组名时，是否让umask的组位等于用户位
# 若userdel组的唯一成员，是否删除组
USERGROUPS_ENAB yes

# 是否自动建立家目录
# CREATE_HOME yes
```
## logrotate.conf 滚动日志配置
```py
weekly # 一周一次
rotate 4 # 保留4个
create # 建立新日志（在postrotate前）
dataext # 使用日期作为文件名后缀，否则数字（越大越旧）
#compress # 取消注释则压缩
include /etc/logrotate.d # 包含其他配置

日志1
日志2
{
    missingok # 文件不存在时不报错
    notifempty # 不滚动空文件
    minsize 1M # 达到时间且超过1M才滚动
    size 1M # 超过1M滚动，和时间选项冲突
    create 0664 root utmp # 权限、所有者、所属组
    prerotate
        命令1
        ...
    endscript # 结束prerotate
    sharedscripts # 日志1、2都滚动完才执行，否则分别执行
    postrotate
        ...
    endscript
}
```
## man_db.conf或manpath.config man-db配置
## mdadm.conf RAID配置
```
ARRAY /dev/md? UUID=块设备或分区共同UUID
```
* UUID可以是`mdadm -D /dev/md? | grep -i uuid`得到的`:`形式，或`blkid /dev/sd?`得到的`-`形式
* 无此文件仍可自动识别RAID设备
## modprobe.d 带选项的内核模块配置目录
## modules-load.d 无选项的内核模块配置目录
## motd 登录后进入登录shell前显示的信息（message of the day）
## mtab 指向`/proc/self/mounts`
## network/interfaces 网卡配置
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
## nologin.txt nologin替代文字（util-linux）
## os-release 发行版信息
## pam.d PAM配置目录
## passwd 用户信息
字段|意义
-|-
1|用户名
2|x，原先密码列
3|UID
4|主GID
5|姓名\[,办公室,办公电话,宅电,其他]
6|家目录
7|登录shell
## quotagrpadmins quota组管理员
```
组名: 用户名
```
## rc?.d SysV各运行等级的服务脚本链接目录
## resolv.conf DNS服务器列表
* 由systemd-resolved或NetworkManager自动生成
## rsyslog.conf rsyslog配置
## securetty root可登录的tty
## security/limits.conf 默认shell资源限制
## selinux/config SELinux配置
```sh
SELINUX=enforcing # permissive或disabled
SELINUXTYPE=targeted # minimum或mls
```
## services 服务名到端口、协议的映射
## shadow 用户密码
字段|意义
-|-
1|用户名
2|加密的密码
3|上次修改密码的时间，1970年1月1日为1
4|PASS_MIN_DAYS
5|PASS_MAX_DAYS
6|PASS_WARN_AGE
7|密码到期后多少天仍可登录
8|账户失效时间
9|保留
## shells 系统中的登录shell
## skel 框架目录
* `useradd -m`默认将该目录下的文件复制到家目录
## ssl/certs 安装的证书目录
## sudoers sudo配置
```
# 用户别名
User_Alias ADMINS = user1, user2

# 命令别名
# 可更改除root外的用户密码
Cmnd_Alias PSWD = !/usr/bin/passwd, /usr/bin/passwd [A-Za-z]*, !/usr/bin/passwd root

# 用户配置
# 第一个ALL为所有主机，第二个为可切换所有用户，第三个为所有命令
root ALL=(ALL) ALL
ADMINS ALL=(ALL) PSWD

# 用户组配置
# 无需输入密码，可执行cat
%sudo ALL=(ALL) NOPASSED:/usr/bin/cat

# 开启sudo日志
Defaults logfile=/var/log/sudo

# 包括其他配置
#includedir /etc/sudoers.d
```
## sysconfig RHEL配置目录
## systemd systemd配置目录
## systemd/logind.conf logind配置
```sh
[Login]
#NAutoVTs=6 # 提供的tty数
#KillUserProcesses=no # 是否在用户会话结束时结束其所有进程（包括nohup）
```
## systemd/system 管理员设置的服务目录
## systemd/system/*.target.wants 实现\*目标需要启动的服务目录
* 包含指向实际服务的软链接
* 优先级高于其他服务目录
## tmpfiles.d 临时文件自动生成、清理的本地配置目录
* 同名覆盖`/usr/lib/tmpfiles.d`下的配置
## udev/rules.d 设备配置规则目录
* .rules规则文件按文件名次序读取，若规则匹配则终止
```sh
# 绑定MAC地址和网卡名
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="MAC地址", NAME="网卡名"
```
## warnquota.conf quota邮件配置
## X11 X Window配置文件目录
# home 普通用户家目录
## 用户名 用户家目录
# lib 函数库目录
* 必须位于根分区
## modules/内核版本/kernel 内核模块目录
### arch 架构相关
### crypto 加密
### drivers 驱动
### fs 可识别的文件系统
### lib 函数库
### net 网络
### sound 声音
## systemd/system 包管理器安装的服务目录
# lib64 64位函数库目录
## ld-linux-x86-64.so.2 运行时动态链接器
# lost+found 存放ext文件系统发生错误时一些丢失的片段
# media 可删除的设备目录
# mnt 挂载的设备目录
WSL中Windows盘位于此
# opt 第三方软件目录
# proc 状态信息目录（虚拟）
## bus 总线目录
## cpuinfo CPU信息
### 查看cpu型号及逻辑处理器数
```sh
grep name /proc/cpuinfo | cut -f2 -d: | uniq -c
```
## devices 设备及其主设备号
## filesystems 已加载到内存中，支持的文件系统
## interrupts 注册的中断
## iomap 物理设备内存映射
## ioports 已注册的IO端口
## kallsyms 内核符号的内存地址
## kcores 物理内存
## loadavg CPU和IO负载
* 过去1、5、10分钟的CPU负载，当前运行进程数/总进程数，最后一个运行的进程
## mdstat RAID状态
## meminfo 内存信息
## modules 加载的内核模块
## mounts 指向`/proc/self/mounts`
## partitions 设备分区信息
## self 指向`/proc/当前进程号`
## self/mounts 挂载的文件系统
## swaps swap分区信息
## sys 可更改的内核功能目录
## sys/kernel/yama/ptrace_scope 进程调试选项
值|意义
-|-
0|同一用户的进程
1|父进程
2|只允许管理员调试
3|不可调试，设置后必须重启才能取消
## uptime 运行时间
* 开机以来的秒数、空闲秒数
## version 内核和编译器版本
## 进程号 进程信息
### cmdline 启动命令
### environ 初始环境变量
### exe 指向可执行文件
### fd 打开的文件目录，包含指向文件的链接
### limits 资源限制
### maps 虚拟内存映射
# root root家目录
# run 服务的运行时数据
## systemd/system 运行时生成的服务目录
## utmp 当前登录用户记录
# sbin 系统命令目录
* 必须位于根分区
# srv 服务数据目录
# sys 内核相关信息目录（虚拟）
# tmp 临时文件目录
# usr UNIX Software Resource目录
## bin 用户命令目录
## include 头文件目录
## lib 应用的函数库、目标文件目录
## lib/tmpfiles.d 临时文件自动生成、清理的系统配置目录
## local 管理员下载的软件目录
## sbin 其他系统命令目录
## share 共享文件目录
### ca-certificates/ 机构证书目录
### doc 软件文档目录
### fonts 字体目录
* 添加字体：新建自体名目录（如Consolas），将C:/Windows/fonts中的字体（通常为*.ttf、\*b.ttf、\*i.ttf、\*z.ttf）复制到目录下
### hwdata 硬件厂商、设备ID对应表目录
* pci.ids
* usb.ids
### man 手册目录
### misc/magic.mgc 文件模式-类型对应数据库
### zoneinfo 时区信息目录
## src 源码目录
# var 变动文件目录
## cache 应用暂存文件目录
## lib 应用数据文件目录
### locate/mlocate locate使用的数据库目录
## lock 锁目录
## log 日志目录
### auth.log或secure 身份认证日志
### boot.log 启动日志
### btmp 失败登录记录
### cron crontab日志
### dmesg 内核缓冲区
### kern.log 内核日志
### lastlog 用户最后登录记录
### maillog 邮件日志
### messages或log/syslog 普通系统活动日志
### wtmp 用户登录记录
## run 指向`/run`
## spool 队列数据目录
## spool/anacron anacron时间戳目录
## spool/cron 用户周期任务目录
