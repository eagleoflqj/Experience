# 安装/升级
```sh
curl -L https://raw.githubusercontent.com/snail007/goproxy/master/install_auto.sh | bash
```
# 运行
## 公共选项
选项|意义
-|-
--forever|fork子进程监控运行
--daemon|后台运行
--log 文件|指定日志文件
-C 证书|指定证书，对不同模式默认proxy.crt或无
-K 秘钥|指定秘钥，对不同模式默认proxy.key或无
## HTTP(S)代理
### 服务器
```sh
proxy http -t tcp -p [IP]:端口 [-T tcp -P [上级代理IP]:端口]
```
### 客户端
全局设置http、https代理，或浏览器设置代理
## 内网穿透
访问服务器端口相当于访问客户端端口
### 桥
```sh
proxy bridge -p [桥IP]:端口
```
### 服务器
```sh
proxy server -P [桥IP]:端口 -p [服务器IP]:端口@:客户端端口
```
### 客户端
```sh
proxy client -P 桥IP:端口
```
