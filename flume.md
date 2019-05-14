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
## 配置sink
```
<AGENT>.sinks.<SINK>.<KEY> = <VALUE>
```
## 配置channel
```
<AGENT>.channels.<CHANNEL>.<KEY> = <VALUE>
```
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
