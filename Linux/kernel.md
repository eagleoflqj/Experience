# Make
## headers 生成头文件
* 位于usr/include
## defconfig 生成默认配置（.config）
## oldconfig 读取配置并提示用户更新（新版内核）选项
## menuconfig 文字菜单配置
## xconfig Qt窗口配置
## vmlinux 编译内核
## bzImage 生成压缩的内核
## modules 编译模块
## modules_install 安装模块
## clean 删除大多数生成文件，保留用于构建外部模块的部分
## mrproper 删除当前配置和所有生成文件
# 开机选项
## init PID=1的进程
* init=/bin/bash：免root密码登录
## quiet 抑制输出
## ro/rw 根分区初始挂载方式
## single 单用户模式
## splash 显示启动画面
