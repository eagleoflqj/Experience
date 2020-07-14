# 系统
## anacron 周期任务
```sh
anacron [-fns|-u] [任务1 ...]
```
选项|意义
-|-
-f|强制执行，不考虑时间戳
-n|忽略延迟，隐含`-s`
-s|串行执行
-u|不执行任务，只更新时间戳
* 不同于crontab，不假设机器持续运行
## at 定时任务
```sh
at [-mv] 时间 # 交互式设定
at 选项
```
时间|
-
now + 3 minutes\|hours\|days\|weeks|
HH:mm\[am\|pm] [YYYY-MM-DD\|March 11]|

选项|意义
-|-
-c 任务号|查看任务脚本
-d 任务号|删除任务，同`atrm`
-l|列出任务，同`atq`
-m|无输出也发送邮件
-v|交互指定任务前先输出执行时间
* 命令的工作目录默认当前目录
* 脱离当前shell，由atd管理
* 若当前shell由`su`得到，则邮件发给执行`su`的用户
## batch 在系统负载低时执行at
```sh
batch
```
## crontab 周期任务
```sh
crontab [-u 用户名] 参数或文件
```
参数|意义
-|-
-l|列出任务
-e|用vi设定任务
-r|删除所有任务

```
分0-59 时0-23 日1-31 月1-12 周0-7 命令
```
格式|意义
-|-
*|所有
数字\[,...]|枚举时间
数字-数字|时间段
*（或数字-数字）/数字|（指定时间段内）时间间隔
* 周与月日不可共存
## chroot 改变根目录执行命令/交互shell
```sh
chroot [选项] 新根目录 [命令 [参数]]
```
选项|意义
-|-
--skip-chdir|不切换工作目录到新根目录
## date 日期时间
```sh
date [选项] [+格式]
```
选项|意义
-|-
无|显示当前时间
-d 字符串|解析时间，字符串精确定义见info
-s 字符串|设置时间
-u|显示UTC时间

格式|意义
-|-
%s|UNIX时间
%Y-%m-%d %H:%M:%S|年月日时分秒

字符串|意义
-|-
yyyy-MM-dd hh:mm:ss|年月日时分秒
2 days[ ago]|2天后/前
@2147483647|UNIX时间
## dmesg 查看/控制内核缓冲区
```sh
dmesg [选项]
```
选项|意义
-|-
无|输出
--read-clear|输出并清空
--clear|清空
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
## free （虚拟）内存使用情况
```sh
free [选项]
```
选项|意义
-|-
-b|Bytes
-k|KiB，默认
-m|MiB
-g|GiB
-s 秒数|间隔刷新
## halt/poweroff/reboot 结束所有进程/关机/重启
```sh
halt/poweroff/reboot [选项]
```
选项|意义
-|-
-f|强制

* `halt`不一定切断电源
## hdparm 查看/设置SATA/IDE硬盘参数
```sh
hdparm [选项] 硬盘
```
选项|意义
-|-
-i|查看启动时获得的硬盘信息
-I|查看当前硬盘信息
-t|测试实际性能
-T|测试缓存性能
## init 更改运行等级
```sh
init 等级
```
等级|意义
-|-
0|关机
1|单用户
2|多用户无网络，通常落入3
3|多用户
4|未定义
5|多用户+图形界面
6|重启
## last/lastb 查看成功/失败登录
```sh
last/lastb [选项] [用户1 ...]
```
选项|意义
-|-
-n 行数|显示的行数
-s 时间|起始时间
-t 时间|终止时间
-p 时间|指定时间

时间|
-|
YYYYMMDDhhmmss|
\[YYYY-MM-DD\] \[hh:mm\[:ss\]\]|
now|
yesterday|
today|
tomorrow|
+5min|
-5days|
* 每次系统重启时记录伪用户reboot
## lastlog 查看用户最后登录时间
```sh
lastlog [选项]
```
选项|意义
-|-
-b 天数|晚于指定天数
-t 天数|早于指定天数
-u 用户|指定用户
## ldd 查看程序依赖库
```sh
ldd 程序
```
## logger 记录日志
```sh
logger [-p FACILITY.PRIORITY] 消息
```
* `-p`默认user.notice
## lsb_release 查看LSB信息
```sh
lsb_release [选项]
```
选项|意义
-|-
无或-v|版本
-a|全部
## lsinitrd 查看initramfs内容
```sh
lsinitrd [文件]
```
* 不指定文件则为默认镜像
## lsusb 显示usb设备
```sh
lsusb
```
## lspci 显示PCI总线上设备
```sh
lspci [选项]
```
选项|意义
-|-
-s \[总线:\]\[设备\]\[.功能\]|过滤设备
-v\|-vv\|-vvv|显示详情
## runlevel 查看运行等级
```sh
runlevel
```
* 输出上一个运行等级和当前运行等级
* 为兼容SysV init，systemd将目标映射到运行等级；由于运行等级唯一而目标不唯一，映射只是近似的
* 无法确定的等级为`N`，两个均无法确定输出`unknown`

等级|目标
-|-
0|poweroff
1|rescue
2 3 4|multi-user
5|graphical
6|reboot
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
shutdown [选项] [时间] [消息]
```
选项|意义
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
strace -p 进程号
```
## sync 将内存中的修改写回硬盘
```sh
sync
```
## uname 查看系统信息
```sh
uname [选项]
```
选项|意义
-|-
无或-s|内核名
-a|全部
-i|硬件平台（不可移植）
-m|机器架构
-p|处理器类型（不可移植）
-r|内核版本
## updatedb 更新locate使用的数据库
```sh
updatedb
```
## update-rc.d 更新SysV各运行等级的服务
```sh
update-rc.d 服务 命令
```
命令|意义
-|-
remove|清空`/etc/rc*.d`下的软链接
defaults|依照`/etc/init.d/服务`在`/etc/rc*.d`下添加软链接（需要先remove）
## uptime 查看系统运行时间
```sh
uptime
```
## vmstat 系统资源使用情况
```sh
vmstat [选项] [间隔秒 [计数]]
vmstat -f|-s
```
选项|意义
-|-
-a|显示inact/active，而非buff/cache
-d|硬盘模式
-f|显示fork的进程数
-p 分区|分区模式
-s|事件计数与内存统计
-S 单位|k=1000，K=1024
* 第一行为开机以来的统计，后面为瞬时信息

默认模式标题|意义
-|-
r|运行或等待运行的进程数
b|不可打断睡眠中的进程数
swpd|已使用的swap
free|空闲内存
buff|被用于缓冲的内存
cache|被用于缓存的内存
inact|不活跃内存
active|活跃内存
si|读取的swap
so|写入的swap
bi|读取的块
bo|写入的块
in|每秒中断数
cs|每秒上下文切换数
us|用户态机时占比
sy|内核态机时占比
id|空闲机时占比
wa|等待IO机时占比
st|作为虚拟机，被其他虚拟机偷走的机时（KVM支持）

硬盘模式标题|意义
-|-
total|读/写完成数
merged|合并成一次IO的读/写数
sectors|读/写的扇区数
ms|读/写所花毫秒数
cur|当前进行的IO数
s|IO消耗的秒数
## w 查看在线用户及其运行程序
```sh
w
```
## who 查看在线用户
```sh
who
```
