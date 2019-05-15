# 安装
官网下载解压
# 配置
## 声明agent的组件
```
<AGENT>.sources = <SOURCE 1> ... <SOURCE n>
<AGENT>.sinks = <SINK 1> ... <SINK n>
<AGENT>.channels = <CHANNEL 1> ... <CHANNEL n>
```
## 配置source
```
<AGENT>.sources.<SOURCE>.<KEY> = <VALUE>
```
### Avro
KEY|VALUE
-|-
type|avro
bind|绑定的域名或IP
port|绑定的端口
### Kafka
KEY|VALUE
-|-
type|org.apache.flume.source.kafka.KafkaSource
kafka.bootstrap.servers|`,`间隔的kafka broker
kafka.consumer.group.id|所属consumer group，默认flume
kafka.topics|`,`间隔的kafka topic
kafka.topics.regex|正则形式的kafka topic，优先级高于kafka.topics
batchSize|一个batch中向channel写入消息数目最大值
batchDurationMillis|一个batch写入channel前的最长等待毫秒数
### NetCat TCP
KEY|VALUE
-|-
type|netcat
bind|绑定的域名或IP
port|绑定的端口
max-line-length|最大行字节数，默认512
ack-every-event|对每条消息返回OK，默认true
## 配置interceptor
```
<AGENT>.sources.<SOURCE>.interceptors = <INTERCEPTOR 1> ... <INTERCEPTOR n>
<AGENT>.sources.<SOURCE>.interceptors.<INTERCEPTOR>.<KEY> = <VALUE>
```
### Search and Replace
KEY|VALUE
-|-
type|search_replace
searchPattern|正则表达式
replaceString|替换字符串，可为空
## 配置sink
```
<AGENT>.sinks.<SINK>.<KEY> = <VALUE>
```
### Avro
KEY|VALUE
-|-
type|avro
hostname|输出的域名或IP
port|输出的端口
### Logger
KEY|VALUE
-|-
type|logger

默认配置conf/log4j.properties，可用-D覆盖
```python
flume.root.logger = INFO,LOGFILE # DEBUG,console
flume.log.dir = ./logs
flume.log.file = flume.log
```
## 配置channel
```
<AGENT>.channels.<CHANNEL>.<KEY> = <VALUE>
```
### File
KEY|VALUE
-|-
type|file
checkpointDir|checkpoint目录，默认~/.flume/file-channel/checkpoint
useDualCheckpoints|是否使用备份checkpoint，true则必须指定backupCheckpointDir，默认false
backupCheckpointDir|备份checkpoint目录，不能与checkpointDir或dataDirs相同
dataDirs|`,`隔开的数据目录，放置在不同磁盘可提高性能，默认~/.flume/file-channel/data
### Memory
KEY|VALUE
-|-
type|memory
capacity|channel中的event数目最大值，默认100
transactionCapacity|一个transaction中channel从source给sink的event数目最大值，默认100
## 将source和sink绑定到channel
```
<AGENT>.sources.<SOURCE>.channels = <CHANNEL 1> ... <CHANNEL n>
a1.sinks.<SINK>.channel = <CHANNEL>
```
* 一个source可以绑定多个channel
* 一个sink只能绑定一个channel
## 使用环境变量
${变量名}
* 只能在配置文件的值位置使用
* 启动参数需加入-DpropertiesImplementation=org.apache.flume.node.EnvVarResolverProperties
* 环境变量也可以在conf/flume-env.sh设置
# 命令
## help 帮助
```sh
bin/flume-ng help
```
## version 版本
```sh
bin/flume-ng version
```
## agent 启动flume agent
```sh
bin/flume-ng agent [参数]
```
参数|意义
-|-
-c 目录|指定配置目录
-f 文件|指定配置文件
-n agent名|启动agent
* 若指定配置目录，则该目录成为classpath的第一位
