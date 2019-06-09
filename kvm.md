# 检查
## CPU支持
```sh
egrep -c '(vmx|svm)' /proc/cpuinfo # 需要大于0
```
## 硬件加速
```sh
apt install cpu-checker
kvm-ok
```
# 安装
```sh
apt install qemu-kvm virt-manager
```
# 创建
## 命令行
```sh
virt-install 参数
```
通用参数|意义
-|-
-n 名称|实例名
--memory &lt;MEMORY&gt;|内存大小

安装参数|意义
-|-
--vcpu &lt;VCPUS&gt;|虚拟CPU
--cdrom 镜像|安装镜像

设备参数|意义
-|-
--disk &lt;DISK&gt;|存储

其他参数|意义
-|-
-d|debug模式
## 图形界面
```sh
virt-manager
```
