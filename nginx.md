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
