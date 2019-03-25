# 概念
## 镜像
创建docker容器的模板
## 容器
独立运行的一个或一组应用
## 客户端
通过命令行等工具使用docker API与守护进程通信
## 主机
一个物理机或虚拟机，执行守护进程和容器
## 仓库
保存镜像，类似git的版本库；docker hub类似github
# 安装
## Windows 10
启动或关闭Windows功能，开启Hyper-V  
官网登录并下载安装Docker for Windows Installer.exe
# 命令
## docker 查看可用命令
```sh
docker [命令 --help]
```
## images 查看本地镜像
```sh
docker images
```
## search 在hub搜索镜像
```sh
docker search 关键字
```
## pull 从hub载入镜像
```sh
docker pull 镜像[:标签]
```
标签不指定则默认latest
## run 运行容器
```sh
docker run [参数] 镜像[:标签] [命令]
```
参数|意义
-|-
-i|允许标准输入
-t|分配一个伪终端
-d|后台运行容器
-P|自动将容器端口映射到主机端口
-p p参数|手动指定端口映射
p参数：\[主机ip:\]主机端口:容器端口\[/udp\]
## ps 查看当前运行的容器
```sh
docker ps
```
标题|意义
-|-
CONTAINER ID|容器ID的前几位
IMAGE|使用的镜像
COMMAND|容器启动后执行的命令
CREATED|创建时间
STATUS|状态
PORTS|端口映射
NAMES|容器名，若不指定则随机分配
## port 查看容器端口
```sh
docker port 容器标识
```
容器标识指容器ID前几位或容器名，下同
## logs 查看容器标准输出
```sh
docker logs [-f] 容器标识
```
参数|意义
-|-
-f|实时监视
## top 查看容器内进程
```sh
docker top 容器标识
```
## inspect 查看容器详细信息
```sh
docker inspect 容器标识
```
返回json
## stop 停止容器
```sh
docker stop 容器标识
```
## start 启动已停止容器
```sh
docker start 容器标识
```
## rm 移除容器
```sh
docker rm 容器标识
```
必须首先停止容器
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
## rmi 删除镜像
```sh
docker rmi 镜像
```
不影响tag出的镜像
# Dockerfile
```
FROM 镜像  
MAINTAINER 维护者 "邮箱"
RUN 构建命令
EXPOSE 暴露端口
WORKDIR CMD的工作目录
CMD 容器创建后执行的指令
```
