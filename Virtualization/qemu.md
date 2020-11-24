# qemu-img
```sh
qemu-img create 镜像 大小
```
# qemu-system-ARCH
```sh
qemu-system-ARCH [选项] [镜像]
```
选项|意义
-|-
-accel 方式\|help|选择/列出加速方式
-cdrom 镜像|指定安装镜像
-drive file=镜像,format=格式|指定硬盘镜像
-m 大小|内存大小，默认128MiB
-smp 个数|CPU核数
