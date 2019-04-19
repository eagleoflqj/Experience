# 安装
官网提示安装
# 命令
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
* 主进程收到reload信号后，检查配置文件合法性，若非法则仍使用旧配置；若合法则启动新工作进程，并通知老工作进程结束；老工作进程收到结束信号，处理完当前请求后退出
* 日志文件mv后仍会写入，reopen使得在原位置重建并写入日志文件

也可以采用kill方式通过pid结束nginx，pid写在nginx.pid，位置在nginx.conf中指定，一般为/usr/local/nginx/logs或/var/run
```sh
kill -s QUIT 主进程号
```
# 配置
## 格式
* nginx由配置文件中的指令指定的模块组成，指令分为简单指令和块指令
* 简单指令：名称 参数;
* 块指令：名称 参数{指令...}
* 包含其它指令的块指令称为环境，规定所有环境之外的指令位于主环境
* events和http指令位于主环境，server位于http，location位于server
* 注释以#开头，以换行为结尾
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
