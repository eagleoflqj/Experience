# 命令
## chcon 修改上下文
```sh
chcon [-R] [-u 用户] [-r 角色] [-t 类型] 文件
chcon [-R] --reference=参考文件 文件
```
* `-R`：递归
* `--reference`：复制参考文件的上下文
## getenforce 查看SELinux模式
```sh
getenforce
```
## getsebool 查看策略值
```sh
getsebool -a|策略
```
## restorecon 恢复默认上下文
```sh
restorecon [-Rv] 文件
```
* `-R`：递归
* `-v`：显示详情
## sealert setroubleshoot客户端
```sh
sealert -l 本地ID
```
* 本地ID在安装setroubleshoot-server并启动auditd后在`/var/log/message`中获取
* 提供修复问题的建议
## seinfo 策略信息
```sh
seinfo [选项]
```
选项|意义
-|-
无|统计信息
-b|列出布尔策略
-r|列出所有角色
-t|列出所有类型
-u|列出所有用户
## semanage 管理SELinux
### 上下文
```sh
semanage fcontext -l
```
## sesearch 查询策略
```sh
sesearch -A [选项]
```
选项|意义
-|-
-b 策略|指定策略
-s 主体|指定主体
-t 类型|指定类型
## setsebool 设置策略值
```sh
setsebool [-P] 策略1=0或1 [...]
```
* `-P`：写入配置文件
## sestatus SELinux状态
```sh
sestatus [-bv]
```
* `-b`：显示策略布尔值
* `-v`：显示`/etc/sestatus.conf`列出的程序和文件的上下文
## setenforce 设置SELinux模式
```sh
setenforce Enforcing|Permissive|1|0
```
* 0为Permissive，1为Enforcing
