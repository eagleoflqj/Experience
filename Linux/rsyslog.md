# 配置
## ubuntu风格
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
## centos风格
```sh
$ModLoad imudp
$InputUDPServerRun 514
```
## 规则
* selectors 空白符 action，selectors为;隔开的selector，大小写不敏感
* selector：facilities.\[前缀\]priority，facilities为,隔开的facility，取值参见syslog(3)，不含前缀log_
* priority取值参见`syslog.h`，不含前缀log_，none表示该facility的任何级别不适用此行规则

前缀|意义
-|-
无|本级和高级
=|本级
!|低级
!=|非本级

action|意义
-|-
\[-\]日志文件|前缀-表示不对每条消息强制写回磁盘
用户名|用户
*|当前在线的所有用户
@\[@]主机|远程主机的rsyslog，@为UDP，@@为TCP
stop|丢弃消息
* 若有多个action，则第二个action开始每个以&空格为前缀独占一行
* selector：:property, [!]compare-operation, "value"，根据属性匹配消息
