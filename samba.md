# 安装
```sh
apt install samba
```
# 配置
/etc/samba/smb.conf
```
[共享名]
path = 目录
browseable = yes
read only = no
valid users = 用户1[,...,用户n]
```
```sh
smbpasswd -a 用户
```
* 用户应为已存在的Linux用户
```sh
service smbd restart
```
# 访问
## Windows
资源管理器地址栏`\\主机\共享名`
