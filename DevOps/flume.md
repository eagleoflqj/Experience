# 安装
官网下载解压
# 配置
## 声明agent的组件
```
<AGENT>.sources = <SOURCE 1> ... <SOURCE n>
<AGENT>.sinks = <SINK 1> ... <SINK n>
<AGENT>.channels = <CHANNEL 1> ... <CHANNEL n>
<AGENT>.sinkgroups = <SINKGROUP 1> ... <SINKGROUP n>
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
## 配置sinkgroup
```
<AGENT>.sinkgroups.<SINKGROUP>.<KEY> = <VALUE>
```
KEY|VALUE
-|-
sinks|空格隔开的sink
processor.type|failover或load_balance，默认default
### Default
只接受一个sink，相当于没有sinkgroup
### Failover
* Failover Sink Processor维护一个sink的优先队列，保证只要有一个sink存活则event能被处理
* 崩溃的sink被放入冷却池中并赋予一个随重试失败次数增加的冷却期，当它成功处理一个event后被重新放入存活池
* 每个sink被赋予一个唯一优先级，若最高优先级sink崩溃，次高优先级将处理event
* 若优先级未指定，则按配置中出现的顺序

KEY|VALUE
-|-
processor.priority.&lt;SINK&gt;|优先级，越高越优先
processor.maxpenalty|最大冷却时间毫秒数，默认30000
### Load Balance
* 若启用backoff，sink processor将冷却崩溃的sink一定时间，时间到后若仍崩溃则冷却时间指数增长
* 若关闭backoff，round-robin模式下崩溃sink的负载将传给下一个sink，造成负载不均
* 小数据量下，round-robin模式不会严格执行

KEY|VALUE
-|-
processor.backoff|是否开启指数冷却时间，默认false
processor.selector|round_robin或random，默认round_robin
processor.selector.maxTimeOut|最大冷却时间毫秒数，默认30000
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
