# 终端
## ~ 家目录
```sh
cd ~
cd ~用户名
```
## !! 上一条命令
```sh
sudo !!
PYTHONIOENCODING=utf-8 !!
```
## $? 上一条命令的返回值
```sh
echo $?
```
## <<EOF 输入子进程，直到遇到EOF
```sh
cat <<EOF >文件
内容
EOF
```
## bash Bourne-Again SHell
```sh
bash [参数]
```
参数|意义
-|-
无|启动非登录bash
文件|在新bash中执行脚本
-c 命令字符串 [$0 ... $n]|在新bash中执行命令
-l或--login|启动登录bash
* 登录shell：`$0`（以及`ps -f`的CMD列）以`-`开头或，`login`、`ssh`、tty1~6
* 非登录shell：shell中启动shell、GUI启动终端
* `/etc/passwd`每行最后一项指定登录shell
* `$SHELL`指明当前shell
* 登录bash执行`~/.bash_profile`（CentOS）或`~/.profile`（Ubuntu），一般会在上述文件加入对`~/.bashrc`的执行
* 登录bash退出执行`~/.bash_logout`
* 非登录bash执行`~/.bashrc`
## bc 计算器
```sh
bc
```
### 3位小数
scale=3
### 退出
quit
## cal 日历
```sh
cal [[月] 年]
```
## clear 清屏
```sh
clear
```
## cowsay 奶牛说话
```sh
cowsay 文字
```
## date 日期时间
```sh
date [+%Y-%m-%d\ %H:%M:%S]
```
## echo 输出并换行
```sh
echo 文字
echo $变量名
echo ${变量名}
echo "abc${变量名}def"
```
### 转义
* 无引号：2n、2n+1个\\输出为n个\\
* 单引号：n个\\输出为n个\\
* 双引号：2n-1、2n个\\输出为n个\\
* -e：将无-e时的输出进行转义
## env 列出当前环境便量
```sh
env
```
## exit 退出当前shell
```sh
exit
```
## export 设置环境变量
```sh
export 变量名=值
```
* 环境变量可供子进程访问，但子进程对其的修改不影响父进程
* export后第二次可以直接赋值
## login 登录账户
```sh
login 用户名
```
## logout 登出当前账户
```sh
logout
```
需要作用在顶层shell或者login后，等同于exit
## man 查看命令手册
```sh
man [参数] 命令
```
参数|意义
-|-
-f|更多相关信息
-k|关键字查找
数字|指定手册条目

数字|意义
-|-
1|用户在shell环境中可以操作的命令或可执行文件
2|系统内核可调用的函数与工具等
3|一些常用的函数与库，大部分为C函数库libc
4|设备文件的说明，通常在/dev下
5|配置文件或某些文件的格式
6|游戏
7|惯例与协议等，例如Linux文件系统、网络协议、ASCII code等说明
8|系统管理员可用的管理命令
9|跟kernel有关的文件

标题|意义
-|-
NAME|简短的命令、数据名称说明
SYNOPSIS|简短的命令执行语法简介
DESCRIPTION|较完整的说明
OPTIONS|针对SYNOPSIS中所有可用的选项说明
COMMANDS|当程序执行时可在其内部执行的命令
FILES|程序使用、参考或连接到的文件
SEE ALSO|其他说明
EXAMPLE|参考范例
BUGS|相关错误
## pwd 输出当前目录
```sh
pwd
```
## su 切换到root
```sh
su
```
## source 在当前shell执行脚本
```sh
source 脚本
```
# 文本
## cat 输出内容
```sh
cat 文件
```
### 合并文件
```sh
cat 文件1 [... 文件n] >新文件
```
## dd 字节复制
```sh
dd [参数]
```
参数|意义
-|-
if=文件|指定输入文件，否则为stdin
of=文件|指定输出文件，否则为stdout
bs=大小|指定一次读取写入量，否则为512字节
### 制作启动盘
```sh
dd if=iso文件 of=/dev/sd? bs=4M
```
## head 查看文件头部
```sh
head [参数] 文件
```
默认前10行
参数|意义
-|-
-行数|显示前几行
-n 行数|显示前几行
-n -行数|不显示最后几行
## grep 查找符合条件的行
```sh
grep [参数] 正则表达式 文件
```
参数|意义
-|-
-n|显示行号
-r|递归查找
-c|只显示匹配行数
-i|忽略大小写
-v|选择不匹配的行
## less 高级翻页查看
```sh
less [参数] 文件1 [... 文件n]
```
参数|意义
-|-
-m|显示百分比
-N|显示行号

按键|意义
-|-
b|后退1屏
f|前进1屏
Enter或j|前进1行
k|后退1行
g|到第1行
G|到最后1行
/字符串|向前查找字符串
?字符串|向后查找字符串
n|下一个
N|上一个
:p|切换到下一个文件
:n|切换到上一个文件
:x|切换到第1个文件
:d|从列表移除当前文件
## more 翻页查看
```sh
more [参数] 文件
```
参数|意义
-|-
+行数|从第几行开始显示
+/字符串|从第一次匹配到字符串的前2行开始显示

按键|意义
-|-
Enter|前进1行
Space|前进1屏
b|后退1屏
=|输出当前行号
:f|输出文件名和当前行号
q|退出
## od 查看字节
```sh
od [参数] [... 文件n]
```
参数|意义
-|-
-o|八进制，默认
-d|十进制
-x|十六进制
-t 进制符\[组字节数\]|指定进制和每组字节数
-A 进制符|指定偏移量进制
--endian=big或little|指定每组多字节时的大小端
-j 字节数|跳过指定字节数
-N 字节数|显示指定字节数
-v|详细模式，重复行不用*代替

未指定文件，则使用标准输入，Ctrl+D结尾
## sort 排序输出内容
```sh
sort [参数] 文件1 [... 文件n]
```
默认字典升序
参数|意义
-|-
-n|数值序
-r|降序
-u|去重
## tac 倒序输出内容
```sh
tac 文件
```
## tail 查看文件末尾
```sh
tail [参数] 文件
```
默认最后10行
参数|意义
-|-
-f|实时监视
+行号|从第几行开始显示
-行数|显示最后几行
-n 行数|显示最后几行
## truncate 调整文件大小
```sh
truncate [参数] [-s 大小 文件]
```
* 原文件较大则删去结尾，较小则补空字符，不存在则新建
* 大小默认字节数，k、m为1024幂，KB、MB为1000幂

参数|意义
-|-
-c|不新建文件
--help|显示帮助
# 角色
## chgrp 设置所有组
```sh
chgrp [参数] 用户组名 文件
```
参数|意义
-|-
-R|递归设置
## chown 设置所有者
```sh
chown [参数] 用户名[:用户组名] 文件
```
参数|意义
-|-
-R|递归设置
## groupadd 添加用户组
```sh
groupadd 组名
```
## groupdel 删除用户组
```sh
groupdel [-f] 组名
```
-f强制删除，组内用户所属组只有组编号
## groupmod 修改用户组
```sh
groupmod -n 新组名 组名
```
## groups 查看用户所属用户组
```sh
groups [用户名]
```
默认当前用户  
输出用户名:用户组
## passwd 设置密码
```sh
passwd [用户名]
```
默认为当前用户
## useradd 添加用户
```sh
useradd [-g 组名] [-m] 用户名
```
* -m自动建立用户目录/home/用户名
* 默认组与用户同名，若同名组已存在则必须指定组名
* 默认登录shell是/bin/sh，上箭头显示^[[A，需将登录shell改为/bin/bash
* 禁止登录shell是/sbin/nologin
## userdel 删除用户
```sh
userdel [参数] 用户名
```
参数|意义
-|-
-f|强制删除
-r|同时删除用户目录、mail spool

被删用户已登录时，可以强制删除，但不会自动登出，新建的文件没有用户名只有用户编号  
被删用户所属文件的所有者变为编号，若新增用户的编号等于此编号，则继承此文件  
被删用户若与所属用户组同名且是组里唯一用户，会同时删除用户组
## usermod 修改用户
```sh
usermod [参数] 用户名
```
参数|意义
-|-
-aG 组名|添加组
-g 主组|修改主组
-l 用户名|修改用户名
## visudo 编辑sudo配置文件
```sh
visudo
```
### 开启sudo日志
```
Defaults logfile=/var/log/sudo
```
### 设置sudo用户组
```
%sudo ALL=(ALL) ALL
```
第一个ALL为所有主机，第二个为所有用户，第三个为所有命令
# 权限
## 文件权限
### r 读取文件
包括用解释器执行文件
### w 写入文件
### x 执行文件
./形式执行
## 目录权限
### r 列出目录下的文件
### w 修改目录
新建文件、重命名文件、删除文件（无论文件所有者与权限）
### x 进入目录
## chmod 设置权限
普通用户只能设置自己的文件  
u所有者 g用户组 o其他人 a所有
r4 w2 x1  
=设置权限 +授予权限 -收回权限  
### 惯例
```sh
chmod 755 目录
chmod 644 文件
```
### 皆可读
```sh
chmod ugo+r 文件
chmod a+r 文件
```
### u可执行，g只读，o不可写
```sh
chmod u+x,g=r,o-w 文件
```
### 递归设置权限
```sh
chmod -R 644 * # 当前文件夹下的文件
chmod -R 644 . # 当前文件夹及其文件
# chmod -R 000 / # 禁令
```
## umask 设置默认权限
```sh
umask [参数]
```
参数|意义
-|-
无|查看当前掩码
-S|查看当前权限
0abc|设置新目录权限777-abc，新文件权限666-abc

可写入~/.bashrc实现持久化
# 重定向
可以解决nohup.out过大的问题  
0标准输入 1标准输出 2标准错误 不写默认1  
\>覆盖 \>\>追加
### 只显示错误
```sh
命令 >/dev/null
```
### 全不显示
```sh
命令 1>/dev/null 2>/dev/null 
```
或
```sh
命令 1>/dev/null 2>&1
```
# 前后台作业
前台程序运行中，Ctrl+Z暂停
## jobs 查看全部作业
```sh
jobs [参数]
```
参数|意义
-|-
-l|显示进程号
带+的是fg、bg、disown默认的作业，带-的是下一个默认的作业
## bg 后台运行作业
```sh
bg [%作业号1 [... %作业号n]]
```
## fg 前台运行作业
```sh
fg [%作业号]
```
关闭终端时，前台、后台、暂停进程收到sighup信号结束  
exit当前shell时，T进程结束；R、S进程（显然为后台进程）被接管，遇到IO时若终端已关闭（TTY为?）则结束，若未关闭则I阻塞，O输出到终端  
## disown 忽略sighup
```sh
disown -h [%作业号或进程号]
```
关闭终端或exit当前shell时，后台R、S进程被接管，其他进程结束
# 目录
## cd 切换目录
```sh
cd 目录
```
### 转到用户目录
```sh
cd
```
### 转到上级目录
```sh
cd ..
```
## cp 复制
```sh
cp [参数] 源 目的
```
参数|意义
-|-
-r|递归复制

源|目的|行为
-|-|-
文件/目录|目录|复制到目录下
文件/目录|目录下新位置|复制
文件|文件|覆盖
目录|文件|非法
## du 查看目录大小
```sh
du [参数] [目录]
```
参数|意义|
-|-
-h|说人话
--max-depth|子目录层数
### 说人话且只显示一层子目录
```sh
du -h --max-depth=1 [目录]
```
## find 查找文件
```sh
find 目录 参数 模式
```
参数|意义|
-|-
-name|文件名
-iname|文件名忽略大小写
## ls 列出文件
```sh
ls [参数] [目录]
```
参数|意义
-|-
-a|包括隐藏的文件
-ct|按修改时间降序
-l|分行显示详细信息
-r|反序
-R|递归输出
-S|按文件大小降序
-X|按扩展名升序
--full-time|显示完整时间

类型|意义
-|-
-|文件
d|目录
l|链接
b|块设备
c|字符设备
### 输出python脚本
```sh
ls *.py
```
## mkdir 创建目录
```sh
mkdir [参数] [目录]
```
参数|意义
-|-
-p|自动创建不存在的父目录
## mv 移动或重命名
```sh
mv 文件 新文件
```
## popd 弹出栈中目录
```sh
popd
```
## pushd 压栈当前目录并跳转
```sh
pushd 新目录
```
## tar 压缩解压
```sh
tar 参数 压缩文件 [被压缩文件]
```
参数|意义
-|-
-c|压缩
-x|解压
-z|使用gzip
-v|输出细节
-f|后跟压缩文件
## rm 删除
```sh
rm [参数] 文件或目录
```
参数|意义
-|-
-f|强制删除
-r|递归删除
## scp 远程复制
```sh
scp [参数] [前缀]源 [前缀]目的
```
参数|意义
-|-
-r|递归复制

前缀：用户名@ip地址:
## which 命令程序位置
```sh
which 命令
```
## /bin 单用户维护模式下能执行的命令目录
## /boot 开机使用的文件目录
## /boot/grub grub引导装载程序文件目录
## /boot/vmlinuz* Linux内核文件
## /dev 设备文件目录
## /dev/hd* IDE盘
## /dev/log rsyslog的套接字
## /dev/nvme* NVMe盘
## /dev/sd* SATA盘
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
## /proc/进程号 进程信息
### 查看输入输出信息
```sh
ls -l fd
```
### 查看启动命令
```sh
cat cmdline
```
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
## /var/run 服务的PID目录
## /var/spool 队列数据目录
## /var/spool/cron 工作排程数据目录
# 进程
## free （虚拟）内存使用情况
```sh
free [参数]
```
参数|意义
-|-
-b|Bytes
-k|KB，默认
-m|MB
-g|GB
-s 秒数|间隔刷新
## kill 向进程发送信号
```sh
kill [参数] 进程号
```
参数|意义
-|-
-s 信号|指定信号
-9|强行结束进程
--|结束进程树，进程号前加-
kill父进程，子进程会被pid=1的init接管
## mono 执行.NET程序
```sh
mono exe程序
```
## nice 更改优先级执行
```sh
nice [-n 相对优先级] 命令
```
相对优先级-20~19，默认10，越低越优先，非管理员不得赋予负值
## nohup 忽略sighup执行
```sh
nohup 命令 [&]
```
遇到input则结束，输出和错误均定向至nohup.out  
nohup和&同时使用时，关闭终端或exit当前shell不结束程序，只是父进程从bash变成了init
## pkill 查找进程并发送信号
```sh
pkill [参数]
```
参数|意义
-|-
-F 文件|指定存放pid的文件
## ps 列出用户进程
```sh
ps [参数]
```
参数|意义
-|-
-e|所有进程
-f|全格式
-l|长格式
-o 小写列名1,...|指定列
-p &lt;PID 1&gt; ...|指定pid
--ppid &lt;PPID 1&gt;|指定ppid

* 行的指定是or关系

标题|意义
-|-
F|标志
S|状态
UID|用户ID
PID|进程号
PPID|父进程号
C|CPU使用率
PRI|调度优先级，数值越小越优先
NI|Nice值，对基准优先级的调整
ADDR|通常情况下，包含进程栈的段号；如果为内核进程，那么为预处理数据区的地址
SZ|占用内存KB
WCHAN|wait channel，正在因为哪个内核函数而休眠
STIME|启动时间
TTY|用户终端位置
TIME|CPU时间
CMD|启动命令

标志|意义
-|-
1|forked but didn't exec
4|used super-user privileges

状态|意义
-|-
D|Uninterruptible sleep (usually IO)
R|Running or runnable (on run queue)
S|Interruptible sleep (waiting for an event to complete)
T|Stopped, either by a job control signal or because it is being traced.
W|paging (not valid since the 2.6.xx kernel)
X|dead (should never be seen)
Z|Defunct ("zombie") process, terminated but not reaped by its parent.
前后台运行中未sleep：R  
前后台运行中sleep、前台等待input：S  
后台等待inpup、前台Ctrl+Z：T
## top 任务管理器
```sh
top
```
## ulimit shell资源限制
```sh
ulimit [参数 [数值或unlimited]]
```
参数|意义
-|-
-a|显示全部资源限定
-c|core文件块数
-d|数据段KB数
-e|nice优先级
-f|创建的文件块数
-i|阻塞信号数
-l|进程锁在内存KB数
-m|使用内存KB数
-n|同时打开文件数
-p|管道512B数
-q|POSIX消息队列字节数
-r|实时nice优先级
-s|栈KB数
-t|CPU秒数
-u|用户进程数
-v|虚拟内存KB数
-x|文件锁数
# 系统
## blkid 查看各分区UUID和类型
```sh
blkid
```
## crontab 定时任务
```sh
crontab [-u 用户名] 参数或文件
```
参数|意义
-|-
-l|列出定时任务
-e|用vi设定任务
-r|删除所有任务

```
分0-59 时0-23 日1-31 月1-12 周几0-7 命令
```
格式|意义
-|-
*|所有
数字[... ,数字]|枚举时间
数字-数字|时间段
*（或数字-数字）/数字|（指定时间段内）时间间隔

crontab的所有操作记录在/var/log/cron
## df 查看硬盘使用情况
```sh
df [参数]
```
参数|意义
-|-
-h|说人话
## dmidecode 查看DMI信息
```sh
dmidecode [参数]
```
参数|意义
-|-
-t 数字或关键字|只显示给定类型设备

数字|意义
-|-
4|处理器
9|系统插槽
17|内存设备

关键字|数字
-|-
processor|4
slot|9
## e2fsck 检查ext文件系统
```sh
e2fsck [参数] 分区
```
参数|意义
-|-
-f|强制检查
-p|自动修复
## fdisk 查看设备
```sh
fdisk -l [设备]
```
## lsusb 显示usb设备
```sh
lsusb
```
## lspci 显示PCI总线上设备
```sh
lspci
```
## mkfs 格式化分区
```sh
mkfs[.文件系统] 分区
```
默认ext2
## mount 挂载分区
```sh
mount 分区 目录
```
若目录非空，旧内容被屏蔽，卸载后可见
## parted 查看分区
```sh
parted -l
```
## service 服务启停
命令将被重定向至systemctl
```sh
service 服务 命令
```
命令|意义
-|-
start|启动
restart|重启
stop|停止
## shutdown 关机
```sh
shutdown [参数] [时间] [消息]
```
参数|意义
-|-
-c|取消
-h|关机
-r|重启

时间|意义
-|-
now|立即
数字|几分钟后
HH:MM|精确时刻
## strace 查看系统调用
```sh
strace 命令
```
## sync 将内存中的修改写回硬盘
```sh
sync
```
## systemctl 管理systemd
```sh
systemctl [命令 [服务]]
```
命令|意义
-|-
start|启动服务
status|服务状态
stop|停止服务
enable|自启
is-enabled|是否自启
disable|禁止自启
daemon-reload|重新加载系统配置
get-default|默认目标
list-dependencies|目标、服务间依赖关系
### 服务.service
一般位于/etc/systemd/system或/usr/lib/systemd/system
```
[Unit]
Description=介绍

[Service]
User=执行用户
Type=forking
WorkingDirectory=工作目录
ExecStart=启动命令

[Install]
WantedBy=目标.target
```
* Type为forking意味着ExecStart的进程fork子进程后退出，子进程成为主进程
* 若没有[Install]，is-enabled输出static，无法自启
* 为自启需要将WantedBy设为默认目标或其依赖；enable在目标的启动服务目录下创建指向此文件的软链接
## tune2fs 文件系统转换
### ext2转ext3
```sh
tune2fs -j 分区
```
### ext3转ext4
```sh
tune2fs -O dir_index,uninit_bg 分区
```
### ext2转ext4
```sh
tune2fs -O dir_index,uninit_bg,has_journal 分区
```
## umount 卸载分区
```sh
umount 分区或目录
```
## uname 查看系统信息
```sh
uname [参数]
```
参数|意义
-|-
-a|全部
-m|硬件平台
-r|内核版本
## update-rc.d 更新各运行等级的服务
```sh
update-rc.d 服务 命令
```
命令|意义
-|-
remove|清空`/etc/rc*.d`下的软链接
defaults|依照`/etc/init.d/服务`在`/etc/rc*.d`下添加软链接（需要先remove）
# 网络
## curl 发送请求
```sh
curl [参数] URL
```
参数|意义
-|-
-d 字符串|指定body
-H 请求头|指定一行header
-X 方法|指定方法
--data-binary @文件|指定body
## dhclient DHCP客户端
```sh
dhclient [网卡]
```
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
## iwlist 查看无线网络
### 扫描无线网
```sh
iwlist [网卡] scan
```
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
ping [参数] 主机
```
参数|意义
-|-
-c 次数|ping给定次，默认无限次
-i 秒数|指定请求间隔，默认1秒
-a|ping的同时响铃（需要终端支持）
## ssh 远程连接
```sh
ssh [参数] 用户名@IP地址
```
参数|意义
-|-
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
## wget 下载文件
```sh
wget URI
```
## 代理
```sh
export http_proxy="[协议]IP:端口"
export https_proxy="[协议]IP:端口"
export no_proxy="[网站,...,网站]"
```
在dns之前决定是否使用代理
