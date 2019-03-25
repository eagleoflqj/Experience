# 终端
## !! 上一条命令
```sh
sudo !!
PYTHONIOENCODING=utf-8 !!
```
## $? 上一条命令的返回值
```sh
echo $?
```
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
无引号时，2n、2n+1个\\输出为n个\\  
单引号时，n个\\输出为n个\\  
双引号时，2n-1、2n个\\输出为n个\\  
-e时，将无-e时的输出进行转义
## exit 退出当前shell
```sh
exit
```
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
-m自动建立用户目录/home/用户名  
默认组与用户同名，若同名组已存在则必须指定组名
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
usermod [-g 新所属组] [-l 新用户名] 用户名
```
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
# 重定向
可以解决nohup.out过大的问题  
0标准输入 1标准输出 2标准错误 不写默认1  
\>覆盖 \>\>追加  
/dev/null无底洞文件
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
## /etc/hosts hosts文件
## /etc/ssl/certs 安装的证书目录
## /home/用户名 用户家目录
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
## /usr/share/ca-certificates/ 机构证书目录
## /usr/share/doc 软件文档
## /usr/share/man 手册目录
## /var/log 日志目录
# 进程
## kill 结束进程
```sh
kill [参数] 进程号
```
参数|意义
-|-
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
## ps 列出用户进程
```sh
ps [参数]
```
参数|意义
-|-
-e|所有进程
-f|全格式
-l|长格式

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
# 系统
## df 查看硬盘使用情况
```sh
df [参数]
```
参数|意义|
-|-
-h|说人话
## lsusb 显示usb设备
```sh
lsusb
```
## lspci 显示PCI总线上设备
```sh
lspci
```
## service 服务启停
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
## strace 查看系统调用
```sh
strace 命令
```
## sync 将内存中的修改写回硬盘
```sh
sync
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

时间|意义
-|-
now|立即
数字|几分钟后
HH:MM|精确时刻
## 查看cpu型号及逻辑处理器数
```sh
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
```
# 网络
## ifconfig 输出网络配置
```sh
ifconfig
```
## ssh 远程连接
```sh
ssh 用户名@IP地址
```
## wget 下载文件
```sh
wget URI
```
