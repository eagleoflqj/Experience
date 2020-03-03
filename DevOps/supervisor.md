# 安装
需要python2
```sh
pip install supervisor
```
# 配置
## 生成默认配置文件
```sh
echo_supervisord_conf > supervisord.conf
mv supervisord.conf /etc/supervisord.conf
```
## 导入任务的配置文件
/etc/supervisord.conf
```
[include]
files = /etc/supervisord.d/*.conf
```
也可以不include，直接写在默认配置中
## 取消任务管理web服务的注释
```
[inet_http_server]
port=9001
username=用户名
password=密码
```
## 在files的目录下添加任务的配置文件
目录不能用~
```python
[program:任务名]
directory=工作目录
command=启动任务的命令
autostart=true # 是否随supervisor的启动而启动，默认true
autorestart=false # 是否自动重启，默认unexpected，即只有异常退出时才重启
stopasgroup=true # 这两项为true用来结束进程树
killasgroup=true
stdout_logfile=目录 # 标准输出位置，默认/tmp/任务名-stdout---supervisor*.log
redirect_stderr=false # 是否将stderr重定向到stdout，默认false
```
# 命令
## 启动服务
```sh
supervisord -c /etc/supervisord.conf
```
## 查看状态
```sh
supervisorctl status
```
## 启动任务
```sh
supervisorctl start 任务名或all
```
## 重启任务
```sh
supervisorctl restart 任务名或all
```
## 结束任务
```sh
supervisorctl stop 任务名或all
```
## 停止服务
```sh
supervisorctl shutdown
```
# web管理
http://localhost:9001/