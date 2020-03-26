# 配置
## 服务.service
一般位于`/etc/systemd/system`或`/usr/lib/systemd/system`
```
[Unit]
Description=介绍

[Service]
User=执行用户
Type=forking
WorkingDirectory=工作目录
ExecStart=启动命令

[Install]
WantedBy=目标.target
```
* Type为forking意味着ExecStart的进程fork子进程后退出，子进程成为主进程
* 若没有\[Install\]，is-enabled输出static，无法自启
* 为自启需要将WantedBy设为默认目标或其依赖；enable在目标的启动服务目录下创建指向此文件的软链接
# 命令
## systemctl 管理systemd
```sh
systemctl [选项] [命令 [单元[.类型]]]
```
命令|意义
-|-
start|启动服务
status|服务状态
stop|停止服务
enable|自启
is-enabled|是否自启
disable|禁止自启
daemon-reload|重新加载系统配置
get-default|默认目标
list-dependencies|目标、服务间依赖关系
list-units|列出各单元
show|查看属性

选项|意义
-|-
-a|显示全部，否则只显示active
-t 类型|device、mount、service、socket、target
-p 属性|配置文件中的属性名，`,`分隔
## systemd-analyze 启动分析
```sh
systemd-analyze [项目] [参数]
```
### time 启动时间
* 固件、内核、initrd、用户空间
* 进入用户空间后，默认目标完成的时间
### blame 各服务初始化时间排序
### critical-chain [目标或服务] 初始化时间依赖
## systemd-resolve 域名解析管理
```sh
systemd-resolve [选项]
```
选项|意义
-|-
--flush-caches|清空缓存
--statistics|查看统计信息
