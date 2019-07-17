# 概念
## 镜像
创建docker容器的模板
## 容器
独立运行的一个或一组应用
* 使用宿主机内核
## 客户端
通过命令行等工具使用docker API与守护进程通信
## 主机
一个物理机或虚拟机，执行守护进程和容器
## 仓库
保存镜像，类似git的版本库；docker hub类似github
## 写时复制
* 多个容器可以共享一个镜像实例，并将各自的改动写在一个可写层中；当删除容器时该层也被删除
* 镜像是在基础镜像上叠加镜像层的文件系统
## Storage Driver
管理容器和镜像层，默认为overlay2
## Data Volume
本地挂载到容器中的目录，不随容器删除而删除，不被Storage Driver管理
# 安装
## Windows 10
启动或关闭Windows功能，开启Hyper-V  
官网登录并下载安装Docker for Windows Installer.exe
## ubuntu
官网按提示apt安装
### 允许普通用户运行docker
```sh
usermod -aG docker 用户名
```
退出重新登录
# 配置
/etc/docker/daemon.json
字段|意义
-|-
data-root|docker主目录，默认/var/lib/docker
dns|DNS服务器列表
insecure-registries|localhost外可用http访问的Registry列表
hosts|监听地址列表，\["fd://","tcp://localhost:2375"\]
log-driver|默认的容器日志驱动，默认json-file
registry-mirrors|docker hub的镜像列表
userland-proxy|是否使用docker-proxy，默认true
* daemon.json和docker.service的启动命令冲突时无法通过systemd启动docker
# 命令
## docker 查看可用命令
```sh
docker [命令 --help]
```
## info 查看容器、镜像、系统等信息
```sh
docker info
```
标题|意义
-|-
Docker Root Dir|docker根目录
## version 查看client、server版本
```sh
docker version
```
## images 查看本地镜像
```sh
docker images [参数]
```
参数|意义
-|-
-f 条件|过滤
-q|只显示镜像ID前12位

条件|意义
-|-
dangling=true|没有标签
label=键=值|label键值
## search 在hub搜索镜像
```sh
docker search 关键字
```
## pull 拉取镜像
```sh
docker pull 镜像[:标签]
```
* 若镜像包括主机前缀则从指定Registry拉取
* 标签默认latest
## run 运行容器
```sh
docker run [参数] 镜像[:标签] [命令]
```
参数|意义
-|-
-c 整数|CPU份额，1~1024
-i|允许标准输入
-t|分配一个伪终端
-d|后台运行容器
-m 空间|物理内存上限
-P|自动将容器端口映射到主机端口
-p p参数|手动指定端口映射
-h 主机名|指定主机名，否则为容器ID前12位
-v 本地目录:容器目录|指定挂载目录
-l 键=值|指定容器标签
--cpuset-cpus 整数|CPU绑定，`0,1-2`
--dns 主机|指定dns服务器
--log-driver 日志驱动|指定日志驱动方式
--mac-address MAC地址|指定MAC地址，默认02:42:ac:11开头
--memory-swap 空间|物理内存+swap上限，不指定则swap上限等于物理内存上限，-1为不限制swap
--name 名称|指定容器名
--read-only|以只读方式挂载/
--restart 重启政策|指定何时重启容器
--rm|退出后删除容器
* p参数：\[主机ip:\]主机端口:容器端口\[/udp\]
* 本地目录可以为目录名（在`data-root/volumes`下自动生成）或宿主机的绝对路径

日志驱动|意义
-|-
none|无日志
json-file|写在`data-root/containers/容器ID/容器ID-json.log`

重启政策|意义
-|-
no|不自动重启，默认
on-failure[:最大尝试次数]|容器异常退出时重启
unless-stopped|自动重启，除非手动关闭容器或docker自身停止/重启
always|总是自动重启
### 内核不支持swap限制
* 修改/etc/default/grub
```sh
GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
```
* `update-grub`
* 重启
## update 更新容器配置
```sh
docker update 参数 容器标识
```
参数|意义
-|-
--restart 重启政策|指定何时重启容器
## rename 重命名容器
```sh
docker rename 容器标识 新名称
```
容器标识指容器ID前几位或容器名，下同
## ps 显示容器
```sh
docker ps [参数]
```
参数|意义
-|-
-a|所有容器，否则只显示运行中的
-f 条件|过滤
-s|显示大小
-q|只显示容器ID前几位

条件|意义
-|-
exited=整数|退出码，-a时有效
label=键=值|label键值

标题|意义
-|-
CONTAINER ID|容器ID的前几位
IMAGE|使用的镜像
COMMAND|容器启动后执行的命令
CREATED|创建时间
STATUS|状态
PORTS|端口映射
NAMES|容器名，若不指定则随机分配
SIZE|文件大小
## port 查看容器端口
```sh
docker port 容器标识
```
## stats 查看容器资源使用
```sh
docker stats [参数] [容器标识]
```
参数|意义
-|-
-a|所有容器，否则只显示运行中的
--no-stream|不刷新
## logs 查看容器stdout、stderr
```sh
docker logs [-f] 容器标识
```
参数|意义
-|-
-f|实时监视
--since 时间|时间起点
--until 时间|时间终点
* CE版只能读取local、json-file、journald
## top 查看容器内进程
```sh
docker top 容器标识
```
* UID列用户名是容器中用户数字ID在宿主机对应的名称，可能与容器不一致
## inspect 查看容器详细信息
```sh
docker inspect 容器标识
```
返回json
### 查看容器IPv4地址
```sh
docker inspect 容器标识 | grep IPAddress
```
## diff 查看容器文件系统的变化
```sh
docker diff 容器标识
```
* A表示增加，C表示修改
## exec 容器内执行命令
```sh
docker exec [参数] 容器标识 命令
```
### 进入容器shell
```sh
docker exec -it 容器标识 /bin/bash
```
## cp 在容器和宿主机间复制
```sh
docker cp [参数] 源 目的
```
* 源和目的一个为`容器标识:容器路径`，一个为`本地路径`
## export 打包容器文件系统
```sh
docker export -o tar文件 容器标识
```
## import 导入容器文件系统
```sh
docker import tar文件 镜像
```
## pause 暂停容器
```sh
docker pause 容器标识
```
## unpause 恢复容器
```sh
docker unpause 容器标识
```
## stop 停止容器
```sh
docker stop [参数] 容器标识
```
参数|意义
-|-
-t 秒数|发送SIGTERM后等待的时间，若还未结束则SIGKILL，默认10
## kill 发送信号
```sh
docker kill [参数] 容器标识
```
参数|意义
-|-
-s 信号|发送的信号，默认KILL
## start 启动已停止容器
```sh
docker start 容器标识
```
## rm 移除容器
```sh
docker rm [参数] 容器标识
```
参数|意义
-|-
-f|强制删除运行中的容器（使用SIGKILL）
-v|删除匿名挂载目录
## wait 等待容器停止，输出退出码
```sh
docker wait 容器
```
## commit 更新镜像
```sh
docker commit -m "commit备注" -a="作者" 容器标识 新镜像[:标签]
```
## build 创建镜像
```sh
docker build -t 镜像[:标签] Dockerfile目录
```
## tag 复制镜像
```sh
docker tag 镜像[:标签] 新镜像[:标签]
```
## push 推送镜像
```sh
docker push 镜像
```
* 若镜像包含主机前缀则推送到指定Registry
## save 打包镜像
```sh
docker save -o tar文件 镜像
```
## load 加载镜像
```sh
docker load -i tar文件
```
## rmi 删除镜像
```sh
docker rmi [选项] 镜像
```
选项|意义
-|-
-f|强制删除
* 不影响tag出的镜像
* 有容器运行时，-f镜像名只去掉镜像tag，-f镜像ID报错
* 只有停止的容器时，-f镜像名或镜像ID都可删除镜像
* 不影响存在的容器
## history 查看镜像历史
```sh
docker history [选项] 镜像
```
选项|意义
-|-
--no-trunc|完整输出
## login 登录docker registry
```sh
docker login [参数] [REGISTRY]
```
将在~/.docker/config.json记录明文密码
参数|意义
-|-
-u 用户名|指定用户名，否则交互式
## logout 退出docker registry
```sh
docker logout [REGISTRY]
```
将删除~/.docker/config.json中的明文密码
## events 查看事件
```sh
docker events [参数]
```
参数|意义
-|-
--since 时间|时间起点
--until 时间|时间终点
# 管理命令
## container
### ls 同ps
## volume 管理`data-root/volumes`下的挂载目录
### create 创建目录
### ls 列出目录
### rm 删除目录
### prune 清理未引用目录
# 日志
* dockerd日志写入`/dev/log`，由syslog读取
# Dockerfile
```Dockerfile
FROM 镜像
MAINTAINER 维护者 "邮箱"
LABEL 键=值
USER 用户名 # 运行后续命令的用户
RUN 构建命令
EXPOSE 暴露端口
WORKDIR 工作目录
VOLUME 挂载点
COPY 本地位置 镜像位置
ENV 环境变量 值
CMD 容器创建后执行的指令
```
* LABEL添加的元信息可用于搜索、识别镜像和容器
* WORKDIR影响其后的命令
* VOLUME指定的挂载点若`docker run`时未指定挂载目录，则在`data-root/volumes`下生成匿名挂载目录
* .dockerignore文件指定不复制进镜像的文件
# 最佳实践
## 保持较小的镜像体积
* 合适的基础镜像，如官方`openjdk`优于`ubuntu`上安装openjdk
* 多阶段构建，最终镜像不包含源码、构建工具等
* Dockerfile中合并命令
* 当一些镜像有很多公共部分时，抽取公共部分制成基础镜像
* 生产镜像作为debug镜像的基础镜像
* 用tag区分镜像的用途和版本，避免依赖默认的latest
## 应用数据持久化
* 使用volume，而不要保存在容器的可写层
