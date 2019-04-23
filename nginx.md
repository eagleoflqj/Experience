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
## 滚动日志
* 首先应重命名日志文件（此时仍能写入）
* 主进程接到USR1信号后，重新打开日志文件，并将其分配给运行工作进程的非特权用户，成功后将它们关闭并向工作进程发送消息
* 工作进程马上打开新文件并关闭旧文件，因此旧文件几乎可以立即被用于后续处理，例如压缩
## 平滑升级
### 启动
* 首先用新版可执行文件替换旧版，然后向主进程发送USR2信号
* 主进程先将nginx.pid重命名为nginx.pid.oldbin，然后启动新主进程，新主进程启动新工作进程，此时所有工作进程同时运行
* 向旧主进程发送WINCH信号可使旧工作进程优雅停止，但旧主进程并未关闭监听套接字
### 回退
若新版未如预期运行，可以用如下两种方式回退
* 先向旧主进程发送HUP信号，旧主进程用旧配置启动工作进程；再向新主进程发送QUIT信号
* 向新主进程发送TERM信号，新工作进程将快速停止（若未停止则应向其发送KILL信号）

无论哪种方式nginx.pid.oldbin都会自动恢复为nginx.pid
### 升级
新版正常运行，则可向旧主进程发送QUIT信号，nginx.pid.oldbin自动删除
# 配置
## 格式
* nginx由配置文件中的指令指定的模块组成，指令分为简单指令和块指令
* 简单指令：名称 参数;
* 块指令：名称 参数{指令...}
* 包含其它指令的块指令称为环境，规定所有环境之外的指令位于主环境
* 注释以#开头，以换行为结尾
## pid
```
pid /var/run/nginx.pid
```
* 位于主环境
* 主进程号存储位置，也可能在/usr/local/nginx/logs下
## http
```
http {
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;
}
```
* 位于主环境
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
* 位于http
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
* 位于server
* location参数可以是路径前缀或以~开头的正则表达式
* root指定了 匹配的URI 的根目录在服务器文件系统的位置
* 用来做反向代理时，proxy_pass指定了接受请求的服务器
## events
* 位于主环境
## use
* 位于events
* 不指定时，nginx会自动选择当前平台最高效的连接处理方法
* select 自动安装在缺乏更高效方法的平台上，源码安装时可用参数指定是否安装
* poll 同上
* kqueue BSD的高效方法
* epoll Linux的高效方法
* /dev/poll Solaris的高效方法
* eventport 同上，但有问题
# 其他
## hash
* 静态资源通过hash散列到桶
* hash表容量由hash_max_size
* 桶容量由hash_bucket_size配置，应为cpu缓存行的倍数
* 如果桶容量等于缓存行大小，那么一次查找最多两次访问内存：计算桶地址和桶内查找，因此hash表扩容时增加hash表容量是首选
