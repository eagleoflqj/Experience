# 安装
官网下载解压
# 命令
## agent 启动agent
```sh
consul agent [参数]
```
参数|意义
-|-
-dev|开发模式
## members 查看集群成员
```sh
consul members [参数]
```
参数|意义
-|-
-detailed|详细信息

members采用gossip协议，保证最终一致性；若要求强一致性，使用HTTP API
```sh
curl localhost:8500/v1/catalog/nodes
```
