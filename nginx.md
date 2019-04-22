# 安装
官网提示安装
# 命令
## nginx
```sh
nginx [参数]
```
参数|意义
-|-
无|启动
-v|查看版本
-s stop|快速停止
-s quit|优雅停止
-s reload|重新加载配置
-s reopen|重新打开日志
* 优雅停止：处理完当前请求再退出
* 配置文件nginx.conf，位于/etc/nginx、/usr/local/nginx/conf或/usr/local/etc/nginx
* 一个主进程，数个可配置个数的工作进程，可以自动调整为计算机核数
* 主进程收到reload信号后，首先检查配置文件合法性，然后尝试应用更改（打开新日志文件和新监听套接字），若失败则仍使用旧配置；若成功则启动新工作进程，并通知老工作进程结束（此时工作进程数可以多于配置）；老工作进程收到结束信号，处理完当前请求后退出
* 为滚动日志，首先应重命名日志文件（此时仍能写入）；主进程接到reopen命令后，重新打开日志文件，并将其分配给运行工作进程的非特权用户，成功后将它们关闭并向工作进程发送消息；使得在原位置重建并写入日志文件
## kill
```sh
kill -s 信号 主进程号
```
信号|意义
-|-
TERM|同stop
INT|同stop
HUP|同reload
USR1|同reopen
USR2|平滑切换到新版本
WINCH|优雅停止工作进程

工作进程也可以接收信号，但实际不需要
信号|意义
-|-
TERM|快速停止
INT|快速停止
QUIT|优雅停止
USR1|重新打开日志
WINCH|为排错而异常终止（需要开启debug_points）
# 配置
## 格式
* nginx由配置文件中的指令指定的模块组成，指令分为简单指令和块指令
* 简单指令：名称 参数;
* 块指令：名称 参数{指令...}
* 包含其它指令的块指令称为环境，规定所有环境之外的指令位于主环境
* events和http指令位于主环境，server位于http，location位于server
* 注释以#开头，以换行为结尾
## pid
```
pid /var/run/nginx.pid
```
主进程号存储位置，也可能在/usr/local/nginx/logs下
## http
```
http {
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;
}
```
* access_log 访问日志，也可位于/usr/local/nginx
* error_log 错误日志，同上
## server
不同server由监听端口listen和服务器名server_name区分
```
server {
    # listen 80;
    root /var/www;
}
```
* listen指定监听端口
* 当location未指定root时，采用server的root
## location
当nginx决定了哪个server去处理请求，它就将请求头中的URI与该server的所有location的前缀参数匹配，记住最长匹配；然后检查正则表达式，如果有匹配的正则就进入该location，否则选取先前记住的
```python
location ~\.(gif|jpg|png)$ {
    root /var/www/images;
}

location / {
    proxy_pass http://localhost:8080;
}
```
* location参数可以是路径前缀或以~开头的正则表达式
* root指定了 匹配的URI 的根目录在服务器文件系统的位置
* 用来做反向代理时，proxy_pass指定了接受请求的服务器
