# 进程
## fuser 列出打开文件/目录的进程
```sh
fuser [选项] [-k [-i] [-信号]] 文件
```
选项|意义
-|-
-i|交互式
-k|发送信号，默认SIGKILL
-m|包括所有打开文件所在文件系统的进程
-u|显示进程所属用户
-v|显示进程详情

Access|意义
-|-
c|进程以该目录为工作目录
e|可执行文件
f|打开的文件
F|打开的文件，写入
m|动态链接库
r|根目录
## killall 向进程发送信号
```sh
killall [选项] [信号] 程序
```
选项|意义
-|-
-e|精确匹配，否则超长程序名只需匹配前15个字符
-i|交互式
-I|忽略大小写
## lsof 列出进程打开的文件
```sh
lsof [选项] [文件1 ...]
```
选项|意义
-|-
-a|AND逻辑，默认OR
-c 程序名前缀|指定程序名
-i \[4\|6]\[TCP\|UDP\]\[@主机\]\[:端口\][-s tcp:状态]|指定网络
-n|NAME列主机名显示为IP，默认域名
-P|NAME列端口显示为端口号，默认监听该端口常用应用名
-p 进程号|指定进程
-u 用户|指定用户
-U|Unix套接字
+c 整数|COMMAND列最长长度，小于len('COMMAND')=7时置为7
+d 目录|指定目录
+D 目录|指定递归目录
* 目录和文件是OR逻辑
## mono 执行.NET程序
```sh
mono exe程序
```
## nice 指定优先级执行
```sh
nice [-n nice值] 命令
```
* nice值-20~19，默认10
* 越低越优先，只有root能指定负值
* 新的子进程继承父进程nice值
## nohup 忽略sighup执行
```sh
nohup 命令 [&]
```
* 遇到input则结束，输出和错误均定向至nohup.out  
* nohup和&同时使用时，关闭终端或exit当前shell，进程变为孤儿
## pidof 程序的PID
```sh
pidof [选项] 程序路径或程序名
```
选项|意义
-|-
-s|只列出一个PID
-x|程序为脚本时也显示PID
## pgrep 查找PID
```sh
pgrep 关键字
```
* 关键字匹配程序名（非命令）
## pkill 查找进程并发送信号
```sh
pkill [参数]
```
参数|意义
-|-
-F 文件|指定存放pid的文件
关键字|指定进程，同`pgrep`
## ps 列出用户进程
```sh
ps [选项]
```
System V选项|意义
-|-
-e|所有进程
-f|全格式
-l|长格式
-H|按PPID树形结构
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
D|不可打断的睡眠（通常由于IO），不接受信号（包括SIGKILL）
R|运行中或在等待队列
S|可打断的睡眠（等待事件完成）
T|暂停，由于作业控制或被调试
W|分页（不存在于2.6之后的内核）
X|死亡，此状态不应出现
Z|僵尸，已终止但因父进程未调用wait，进程表仍保留其表项
* 前后台运行中未sleep：R
* 前后台运行中sleep、前台等待input：S
* 后台等待input、前台Ctrl+Z：T
* 子进程终止，自动向父进程发送SIGCHLD信号
* 父进程终止，子进程变为孤儿，被pid=1的init接管；init周期性调用wait，确保其子进程终止后不会长期处于僵尸状态
## pstree 显示进程树
```sh
pstree [选项] [进程号]
```
选项|意义
-|-
-A|用ASCII字符画树
-p|显示PID
-u|显示与父进程UID的不同
-U|用Unicode字符画树
## renice 调整优先级
```sh
renice nice值 PID
```
* 只有root能调低nice值
## time 统计命令执行时间
```sh
time 命令
```
## top 任务管理器
```sh
top [选项]
```
选项|意义
-|-
-b|不接受input，滚动输出
-d 秒数|设置间隔
-n整数|迭代次数
-p PID|只监控特定进程，该选项可多次指定

命令|意义
-|-
?|列出可用命令
1|总负载/每个CPU负载
k|发送信号
l|显示/隐藏负载
m|切换内存状态显示方式
M|按内存占用率排序
N|按PID排序
P|按CPU使用率排序
q|退出
r|设置nice值
R|反序
t|切换CPU状态显示方式
T|按CPU时间排序
