# 安装
官网下载镜像，烧录U盘，BIOS更改为USB启动
# 配置
## 时间
系统->日期和时间，修改时区，保存
# Raid
* BIOS设置SATA热插拔
* 存储器->RAID管理->创建
* 指定级别和设备，创建，等待同步
# 插件
## omv-extras
omv-extras.org 按照GUIDES指示安装
## zfs
* 系统->插件->Filesystems->openmediavault-zfs
* 若有报错，则执行`/sbin/modprobe zfs`
* 存储器->ZFS->Add Pool
* 选择Pool type、设备，保存
* 选中刚刚创建的Pool，编辑，设置acltype为posixacl
# 共享
## 文件夹
* 访问权限管理->共享文件夹->添加
* 选中刚刚创建的文件夹，点击`ACL`设置Linux权限，其中`用户/组 权限`设置细分权限，`扩展选项`设置ugo及其权限，默认用户组为users
## SMB
* 服务->SMB/CIFS->常规设置->启用，保存
* 共享->添加，选择已有的共享文件夹，设置允许主机/拒绝主机，保存
# 问题
## 升级Debian
```sh
apt update
apt upgrade
sed -i 's/stretch/buster/g' /etc/apt/sources.list
apt upgrade
```
## Mellanox
* 为安装linux-headers，需使用非bpo内核
* 修改mlnxofedinstall，将不在apt源中的低版本软件替换成可用的高版本
