# 配额
## edquota 设置配额（编辑器）
```sh
edquota -u 用户名|-g 组名|-t
edquota -p 用户名1 -u 用户名2 # 将用户1的配额复制给用户2
```
* `-t`：设置宽限期
* blocks：已使用的1KiB块数
## quota 用户/组配额报表
```sh
quota [-gsu] [用户名1/组名1 ...]
```
* `-s`：人类可读*iB单位
* 默认当前用户，组默认当前用户所属所有组
## quotacheck 检查/建立配额配置
```sh
quotacheck [选项] [-a|分区]
```
选项|意义
-|-
-a|检查`/etc/mtab`中所有挂载的非NFS文件系统
-g|只针对组配额，默认只针对用户配额
-m|（危险）不重新挂载，强制检查
-M|（危险）若重新挂载失败，强制检查
-ug|针对用户和组配额
-v|输出详情
* 若文件系统根目录下不存在`aquota.user`/`aquota.group`则新建
* 为防止检查时其他程序写入，尝试以只读方式重新挂载，检查完毕再以读写方式重新挂载
## quotaoff 关闭配额
```sh
quotaoff [-guv] [-a|分区]
```
## quotaon 开启配额
```sh
quotaon [-guv] [-a|分区]
```
## repquota 文件系统配额报表
```sh
repquota [-gsuv] [-a|分区]
```
## setquota 设置配额（命令行）
```sh
setquota [-u|-g] 用户名|组名 块软限制 块硬限制 i节点软限制 i节点硬限制 -a|分区
setquota -t [-u|-g] 块宽限期秒 i节点宽限期秒 -a|分区
```
## warnquota 向超过配额用户/组管理员发送邮件
```sh
warnquota [-g]
```
* 需要本地邮件服务器运行
* 组管理员在`/etc/quotagrpadmins`指定
