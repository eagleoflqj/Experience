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
-c 文件|指定配置文件启动
-v|查看版本
-V|查看版本、编译器版本和安装配置
-s stop|快速停止
-s quit|优雅停止
-s reload|重新加载配置
-s reopen|重新打开日志
-? 或 -h|命令行参数帮助
-g "若干指令"|指定全局配置
-p 目录|设置nginx路径前缀，默认/usr/local/nginx或/usr/share/nginx
-t|不启动，只检查配置文件，并尝试打开配置指明的文件
-T|同-t，并输出所有配置文件
-q|检查配置文件时忽略非error消息
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
旧进程应以绝对路径或service方式启动，不能以依赖环境变量的nginx命令启动，否则kill将报错`execve() failed while executing new binary process "nginx"
(2: No such file or directory)`
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
```nginx
pid /var/run/nginx.pid
```
* 位于主环境
* 主进程号存储位置，也可能在路径前缀/logs下
## http
```
http {
}
```
* 位于主环境
## keepalive_timeout
```nginx
keepalive_timeout timeout [header_timeout];
```
* 位于http、server、location
* timeout是服务器保持连接的时间，默认75s
* header_timeout将在响应头设置`Keep-Alive: timeout=header_timeout`，但就算小于timeout，所有早于timeout连接仍被复用
## access_log
```nginx
access_log /var/log/nginx/access.log;
```
* 位于http、server、location、location中的if、limit_except
* 指定访问日志位置，若是相对位置则相对路径前缀
### 输出到rsyslog
```nginx
access_log syslog:参数,...;
```
### 参数
* server=地址，地址可以为 unix:sock文件 或 主机\[:端口\]，端口默认514
* facility=字符串，字符串由syslog(3)定义，默认local7
* severity=字符串，同上，默认info
* tag=字符串，默认nginx
* nohostname，不显示主机名
## error_log
```nginx
error_log 存储位置 [debug];
```
* 位于主环境、http、mail、stream、server、location
* 指定debug则error包括debug日志，此功能需要源码安装时指定--with-debug，软件源安装自动包含
* 内层指令覆盖外层
* 也可以向内存环形缓冲区输出日志，用于高负载下debug
```nginx
error_log memory:32m debug;
```
用gdb脚本读取内存中的日志
```sh
set $log = ngx_cycle->log

while $log->writer != ngx_log_memory_writer
    set $log = $log->next
end

set $buf = (ngx_log_memory_buf_t *) $log->wdata
dump binary memory debug_log.txt $buf->start $buf->end
```
## server
不同server由监听端口listen和服务器名server_name区分
```nginx
server {
    # listen [IP:]80 [default_server];
    server_name 主机名1 [... 主机名n];
    root /var/www;

    listen 443 ssl;
    ssl_certificate     服务器证书;
    ssl_certificate_key 服务器私钥;
    # ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    # ssl_ciphers HIGH:!aNULL:!MD5;
    # ssl_session_cache none;
    # ssl_session_timeout 5m;
}
```
* 位于http
* listen指定监听IP和端口，默认全部可用IP
* nginx首先根据请求的IP和端口找出匹配的所有server，再匹配请求头Host与这些server的server_name；若无匹配，则选取该（IP）端口default_server，若未指定则取第一个
* 当location未指定root时，采用server的root
* ssl指令可以位于http或server
* 证书是可以被访问的公共实体，私钥应有严格的访问权限，但能被nginx主进程读取；二者也可存于同一个限制访问的文件，只有证书被发送给客户端
* 对于证书链合成的证书文件，服务器证书应出现在最前
* ssl_protocols和ssl_ciphers的默认值用于强制使用SSL/TLS的高版本
* ssl_session_cache和ssl_session_timeout设置服务器缓存ssl会话参数的大小和时间，1MB缓存4000个会话，但我设置后wireshark抓包没体现出区别
* 早期版本不支持SNI：由于SSL先于HTTP，同IP下只有default_server指定的证书被发送给客户端，若要使用不同证书需要不同IP
* 当前版本支持SNI：对于现代浏览器，TLS的client hello阶段包含server_name扩展指明域名；用于构建nginx的新版openssl可以区分域名
* `nginx -V`输出`TLS SNI support enabled`表示nginx支持SNI；但若nginx和一个不支持SNI的openssl库动态链接，nginx将报warning
### 禁止空Host
HTTP/1.1标准禁止空Host，此方法针对HTTP/1.0
```nginx
server {
    server_name "";
    return 444; # 返回非标准状态码444并关闭连接
}
```
## server_name
```nginx
server_name 127.0.0.1;
server_name example.org;
server_name *.example.org;
server_name mail.*;
server_name ~^(?<user>.+)\.example\.net$;
```
* 位于server
* $hostname表示主机名
* 通配符\*只能出现在.隔开的开头或结尾（最多一次），但可以匹配多个.隔开的部分
* .example.org既可作为example.org的全名匹配，也可匹配 www.example.org
* 正则表达式需以~开头，含有{}时整体应置于""中
* 支持捕获?\<name\>、?'name'和?P\<name\>，可在后续用$name引用；$数字 由于顺序不稳定不推荐使用
* 当多个server_name匹配时，按全名、\*开头的最长、\*结尾的最长、最先出现正则的顺序决定server
* 前三类存在三张hashtable中，全名搜索最快，通配符按.隔开部分搜索，正则顺序搜索最慢
* 若example.org和www.example.org访问量大，应将`.example.org`替换为`example.org  www.example.org  *.example.org`
* 若server_name过多或过长，则应增大server_names_hash_max_size和server_names_hash_bucket_size
* 若某端口下只有一个server，则不会尝试匹配server_name（因此也不会建立hashtable），除非有包含捕获的正则表达式
* 国际化域名使用Punycode
## location
当nginx决定了哪个server去处理请求，它就将请求头中的URI（不包括?及参数，因为参数顺序是任意的）与该server的所有location的前缀参数匹配，记住最长匹配；然后检查正则表达式，如果有匹配的正则就进入该location，否则选取先前记住的
```nginx
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
## proxy_set_header
```nginx
# proxy_set_header Host $proxy_path;
# proxy_set_header Connection close;
```
* 位于http、server、location
* 修改发给代理服务器请求的请求头
* 当前环境的配置覆盖父环境的配置
## index
```nginx
index index.html /index.html
```
* 位于http、server、location
* 决定了如何处理以/结尾的请求，将产生内部重定向
* 参数按顺序匹配；最后一个参数可以以/开头，重定向为绝对URI；其余参数遵循root指定的目录
## upstream
```nginx
upstream backend {
    # least_conn;
    # ip_hash;
    server backend1.example.com weight=5;
    server 127.0.0.1:8080 max_fails=3 fail_timeout=30s;
    server unix:/tmp/backend3;

    server backup1.example.com backup;
    server down.example.com down;

    keepalive 16;
}
server {
    location / {
        proxy_pass http[s]://backend;
    }
}
```
* 位于http
* 负载均衡默认round-robin方法，weight指明服务器的权重；宕掉的服务器被自动剔除；若所有服务器均无法响应，backup服务器将处理请求
* max_fails（默认1）和fail_timeout（默认10s）决定了在多长时间内连续多少次被动请求失败将被判定为宕机，之后同样的一段时间将得不到请求
* max_fails设为0则不做宕机判定（无视fail_timeout）
* fail_timeout后的请求若成功，则服务器被重新判定为正常
* least_conn方法使请求交给连接数最少的服务器处理
* 上述方法存在同一用户的不同请求交给不同服务器处理的问题，解决方法之一是使用ip_hash方法
* down表示该服务器不处理请求
* keepalive表示开启与upstream的保持连接，参数指定了每个工作进程与该组的server的总保持连接数（不是总连接数限制）
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
## debug_connection
```nginx
debug_connection 127.0.0.1;
debug_connection localhost;
debug_connection 192.0.2.0/24;
debug_connection ::1;
debug_connection 2001:0db8::/32;
debug_connection unix:;
```
* 位于events
* 指定使用debug log的客户端连接，其它连接服从error_log指定的级别
# 其他
## 变量
### host 按优先级依次为URL的主机、Host头、server_name
### proxy_host proxy_pass中指定的代理服务器地址
## hash
* 静态资源通过hash散列到桶
* hash表容量由hash_max_size
* 桶容量由hash_bucket_size配置，应为cpu缓存行的倍数
* 如果桶容量等于缓存行大小，那么一次查找最多两次访问内存：计算桶地址和桶内查找，因此hash表扩容时增加hash表容量是首选
## 度量单位
### 大小
k、m、g，大小写不敏感，无后缀为字节
### 时间
ms、s、m、h、d、w（7天）、M（30天）、y（365天），大小写敏感，无后缀为秒，可以用 大单位\[空格\]小单位 组合