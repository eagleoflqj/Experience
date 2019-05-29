# 概念
## NRT
Nearly Realtime，文档被索引到可被搜索只要1秒左右
## Cluster
按名区分，一个node只能属于一个cluster
## Node
按名区分，默认加入elasticsearch cluster
## Index
document的集合，按名区分，名称小写
## Document
可被索引的基本单元，json格式
## Shard
一个index，可被独立索引，支持横向扩展和并行加速，其分配和搜索结果聚合方法由ES控制，对用户透明
## Replica
shard在不同node上的备份，支持并行加速，其数目可以动态调整
* 如果cluster有至少2个node，默认每个index有1个primary shard和1个replica shard
# 安装
官网下载解压
# 目录
* config、data、logs应置于$ES_HOME外以便升级
## bin 可执行程序
文件|描述
-|-
elasticsearch|启动es
elasticsearch-plugin|安装插件
## config 配置
文件|描述
-|-
elasticsearch.yml|es配置
jvm.options|jvm选项
log4j2.properties|日志配置
* ES_PATH_CONF可以指定config目录，对于archive方式的安装在命令行指定，对于包管理安装则应在`/etc/default/elasticsearch`（Debian）或`/etc/sysconfig/elasticsearch`（RPM）指定
## data 数据
## plugins 插件
# 配置
## elasticsearch.yml
```yaml
cluster.name: elasticsearch
node.name: ${HOSTNAME}
path.data: ${ES_HOME}/data
path.logs: ${ES_HOME}/logs
network.host: localhost
http.port: 9200
```
* 环境变量${变量名}
## jvm.options
* `-Xmx2g` 所有版本
* `8:-Xmx2g` JDK 8
* `8-:-Xmx2g` JDK 8+
* `8-9:-Xmx2g` JDK 8-9
* 也可使用ES_JAVA_OPTS指定
## log4j2.properties
* ${sys:es.logs.base_path} logs目录位置
* ${sys:es.logs.cluster_name} cluster名
* ${sys:es.logs.node_name} node名
* ${sys:file.separator} Linux下为/
```yaml
######## Server JSON ############################
appender.rolling.type = RollingFile # RollingFile appender
appender.rolling.name = rolling
appender.rolling.fileName = ${sys:es.logs.base_path}${sys:file.separator}${sys:es.logs.cluster_name}_server.json # 最新日志文件名
appender.rolling.layout.type = ESJsonLayout # 使用json
appender.rolling.layout.type_name = server # json中的type域
appender.rolling.filePattern = ${sys:es.logs.base_path}${sys:file.separator}${sys:es.logs.cluster_name}-%d{yyyy-MM-dd}-%i.json.gz # 滚动日志文件名，压缩，i自增
################################################
```
## keystore
* 必须以启动ES的用户来执行程序
* 非reloadable的设置需要重启ES以应用修改
* ES 7.1仅提供混淆，未来将提供密码保护
* 各node的keystore应相同
### 创建
```sh
bin/elasticsearch-keystore create
```
* 将在config下创建elasticsearch.keystore
### 列出key
```sh
bin/elasticsearch-keystore list
```
### 添加key-string
```sh
bin/elasticsearch-keystore add <KEY>
```
### 添加key-file
```sh
bin/elasticsearch-keystore add-file <KEY> <FILE>
```
### 删除key
```sh
bin/elasticsearch-keystore remove <KEY 1> ... <KEY n>
```
# 启动
```sh
bin/elasticsearch [参数]
```
参数|意义
-|-
-d|以守护进程运行
-p 文件|指定存放pid的文件
-q|不输出stdout
-Ecluster.name=&lt;CLUSTER&gt;|指定cluter名
-Enode.name=&lt;NODE&gt;|指定node名
* 一般cluster配置应在elasticsearch.yml指定，node配置应在命令行指定
* jdk目录自带openjdk，也可通过指定JAVA_HOME使用本地jdk，此时可以删除jdk目录
# REST API
* 传送json参数时需指定header：`Content-Type: application/json`，将json作为body
### GET / cluster信息
## _cat
### GET /_cat/health 健康信息
status|意义
-|-
green|cluster正常
yellow|cluster正常但有的replica未分配
red|部分数据不可获取，其他数据可以使用，但必须尽早排除故障
### GET /_cat/indices index信息
### GET /_cat/nodes node信息
## index
### PUT /&lt;INDEX&gt; 创建index
默认1个primary、1个replica，当只有1个node时replica得不到分配，status为yellow
### DELETE /&lt;INDEX&gt; 删除index
## document
### PUT /&lt;INDEX&gt;/_doc/&lt;ID&gt; 加入document
* 若index不存在，则自动创建
* 若ID已存在，则document被覆盖
### POST /&lt;INDEX&gt;/_doc 加入document
* 同PUT，但产生随机ID

响应字段|意义
-|-
_id|随机ID
### GET /&lt;INDEX&gt;/_doc/&lt;ID&gt; 查看document
响应字段|意义
-|-
found|是否存在该ID
_source|document的json
### POST /&lt;INDEX&gt;/_update/&lt;ID&gt; 更新document
请求字段|类型|意义
-|-|-
doc|dict|将要更新的键值
script|str|执行语句，不可与doc并存
### DELETE /&lt;INDEX&gt;/_doc/&lt;ID&gt; 删除document
## _bulk
### POST /&lt;INDEX&gt;/_bulk 批处理
* body是任务构成的json line
* 每个任务第一行为动作，值为`{"_id": "<ID>"}`
* 如果任务单独执行时有body，则body为第二行

动作字段|意义
-|-
index|加入document
update|更新document
delete|删除document
## _search
### GET /&lt;INDEX&gt;/_search
请求字段|类型|意义
-|-|-
query|dict|查询条件
query.match_all|{}|所有document
query.match|dict|查询条件
query.match.&lt;KEY&gt;|int|KEY处VALUE等于定值
query.match.&lt;KEY&gt;|str|KEY处VALUE包含给定value中的任一单词，不区分大小写
query.match_phrase.&lt;KEY&gt;|str|KEY处VALUE包含给定value词组，不区分大小写
query.bool|dict|逻辑运算
query.bool.must|list|相当于query，需要满足所有条件
query.bool.must_not|list|相当于query，需要不满足所有条件
query.bool.should|list|相当于query，需要满足任一条件
query.bool.filter|dict|过滤条件
query.bool.filter.range|dict|按范围过滤
query.bool.filter.range.&lt;KEY&gt;|dict|指定字段
query.bool.filter.range.&lt;KEY&gt;.&lt;gt\|gte\|lt\|lte&gt;|num|指定上下界
sort|list|排序依据
sort.&lt;KEY&gt;|str|asc或desc
from|int|偏移量，默认0
size|int|返回结果数，默认10
_source|list|返回结果的字段，默认全部
aggs|dict|聚集查询
aggs.&lt;NAME&gt;|dict|自定查询名称
aggs.&lt;NAME&gt;.terms|dict|按值分组
aggs.&lt;NAME&gt;.terms.order|dict|排序依据
aggs.&lt;NAME1&gt;.terms.order.&lt;NAME2&gt;|str|按NAME2排序，asc或desc
aggs.&lt;NAME&gt;.range|dict|按区间分组
aggs.&lt;NAME&gt;.range.ranges|list|分组区间
aggs.&lt;NAME&gt;.range.ranges.from|num|下界，含
aggs.&lt;NAME&gt;.range.ranges.to|num|上界，不含
aggs.&lt;NAME&gt;.avg|dict|平均值
aggs.&lt;NAME&gt;.&lt;AGG&gt;.field|str|聚集字段，&lt;KEY&gt;（数值）或&lt;KEY&gt;.keyword（字符串）
aggs.&lt;NAME&gt;.aggs|dict|嵌入聚集

响应字段|意义
-|-
took|查找毫秒数
time_out|是否超时
_shards|被查找的shard信息
hits|查询结果
hits.total|匹配的document总数信息
hits.total.value|数值
hits.total.relation|实际值与数值的关系，eq或gte
hits.hits|结果数组
hits.hits.sort|不按score排序时排序的键
hits.hits._source|结果json
aggregations|聚集结果
aggregations.&lt;NAME&gt;|自定查询名称的结果
aggregations.&lt;NAME&gt;.buckets|聚集结果数组
### GET /&lt;INDEX&gt;/_search?&lt;ARGUMENT&gt;
参数|意义
-|-
q=*|所有document
sort=&lt;KEY&gt;:&lt;asc\|desc&gt;|结果按KEY处VALUE升序/降序
from=&lt;OFFSET&gt;|偏移量，默认0
size=&lt;SIZE&gt;|返回document数，默认10
## _nodes
### POST _nodes/reload_secure_settings 应用reloadable的keystore设置
* 返回响应时所有node已应用新设置
