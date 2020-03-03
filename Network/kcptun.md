# 安装
https://github.com/xtaci/kcptun/releases
# 服务器
```sh
ulimit -n 65535
server_linux_amd64 -t [目标IP]:端口 -l [监听IP]:端口 -mode fast3 -nocomp -sockbuf 16777217 -dscp 46
```
# 客户
```sh
client_darwin_amd64 -r 服务器IP:端口 -l [监听IP]:端口 -mode fast3 -nocomp -autoexpire 900 -sockbuf 16777217 -dscp 46
```
