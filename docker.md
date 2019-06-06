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
dns|DNS服务器列表
graph|docker主目录，默认/var/lib/docker
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
-h 主机名|指定主机名，否则为容器ID前12位
--name 名称|指定容器名

p参数：\[主机ip:\]主机端口:容器端口\[/udp\]
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
-s|显示大小
-q|只显示容器ID前几位

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
## exec 容器内执行命令
```sh
docker exec [参数] 容器标识 命令
```
### 进入容器shell
```sh
docker exec -it 容器标识 /bin/bash
```
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
# 管理命令
## container
### ls 同ps
# 搭建Registry
## 安装
```sh
docker pull registry
```
# Dockerfile
```Dockerfile
FROM 镜像
MAINTAINER 维护者 "邮箱"
RUN 构建命令
EXPOSE 暴露端口
WORKDIR CMD的工作目录
COPY 本地位置 容器位置
ENV 环境变量 值
CMD 容器创建后执行的指令
```
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
