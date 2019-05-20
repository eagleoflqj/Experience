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
# 启动
```sh
bin/elasticsearch [参数]
```
参数|意义
-|-
-Ecluster.name=&lt;CLUSTER&gt;|指定cluter名，默认elasticsearch
-Enode.name=&lt;NODE&gt;|指定node名，默认主机名
* jdk目录自带openjdk，也可通过指定JAVA_HOME使用本地jdk，此时可以删除jdk目录
# REST API
9200端口监听请求
## _cat
### GET /_cat/health 健康信息
status|意义
-|-
green|cluster正常
yellow|cluster正常但有的replica未分配
red|部分数据不可获取，其他数据可以使用，但必须尽早排除故障
### GET /_cat/indices index信息
### GET /_cat/nodes node信息
