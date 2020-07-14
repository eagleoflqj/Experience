# 分区与文件系统
## badblocks 寻找坏块
```sh
badblocks [选项] 分区
```
选项|意义
-|-
-n|非破坏性读写模式，默认读模式
-s|显示进度条
-v|在标准错误输出读错误、写错误、数据损坏数
-w|破坏性写模式
## blkid 查看磁盘/分区UUID和类型
```sh
blkid [磁盘或分区]
```
* 无参数则显示所有分区
## df 查看文件系统使用情况
```sh
df [选项] [文件1 ...]
```
选项|意义
-|-
无|同-k，除非设置了`$POSIXLY_CORRECT`（默认以512字节块为单位）
-a|包括伪、重复、不可访问的文件系统
-h|人类可读*iB单位
-i|不显示块信息，显示i节点信息
-H|同-h，但使用*B
-k|以KiB为单位
-m|以MiB为单位
-T|显示文件系统
* 显示文件所在文件系统，无文件则显示所有挂载的文件系统
## dump 备份
```sh
dump [选项] [-f 备份文件] 文件系统或目录
dump -W
```
选项|意义
-|-
-j\[等级]|bzip2压缩，默认等级2
-S|估计所需空间
-u|更新`/etc/dumpdates`
-v|输出详情
-数字|备份等级，0为全量，正数为对上一等级的增量（需要`/etc/dumpdates`存在上一等级的记录）
-W|列出`/etc/fstab`和`/etc/dumpdates`中文件系统备份情况
* 增量备份、`-u`只使用于文件系统
## dumpe2fs 查看ext文件系统信息
```sh
dumpe2fs [选项] 分区
```
选项|意义
-|-
-b|显示保留为坏道的块
-h|只显示超级块信息，不显示块组信息
## e2fsck 检查ext文件系统
```sh
e2fsck [选项] 分区
```
选项|意义
-|-
-D|优化目录结构
-f|强制检查，即使文件系统看似状态良好
-p或（不推荐）-a|自动修复
* 出现错误的文件被放在`lost+found`下
## e2label 查看/设置ext文件系统卷标
```sh
e2label 分区 [新卷标]
```
## fdisk 操作磁盘分区表
### 交互设置
```sh
fdisk [选项] 磁盘
```
选项|意义
-|-
-c=dos|兼容dos（磁道对齐），否则1MiB对齐
-C 数字|指定柱面数
-H 数字|指定磁头数
-S 数字|指定每磁道扇区数

命令|意义
-|-
a|开/关分区启动标志，只用于dos兼容模式
c|开/关dos兼容模式
d|删除分区
F|列出未分区的空闲区
l|列出所有分区类型
m|列出命令菜单
n|新建分区
p|列出分区表
q|不保存退出
t|修改分区类型
w|保存并退出
x|进入专家模式

专家命令|意义
-|-
c|更改柱面数
h|更改磁头数
m|列出命令菜单
q|不保存退出
r|回到主菜单
s|更改每磁道扇区数
### 输出
```sh
fdisk -l [设备]
```
* 不指定设备则遍历/proc/partitions
## findmnt 列出（挂载的）文件系统
```sh
findmnt [选项] [分区] [目录]
```
选项|意义
-|-
-a|用ASCII字符画树
-m|查找`/etc/mtab`
-s|查找`/etc/fstab`
## fsck 检查、修复文件系统
```sh
fsck [选项] 文件系统
```
选项|意义
-|-
-A|按`/etc/fstab`的要求检查
-C|显示进度条
-t 文件系统|指定文件系统，否则自动检测
* 文件系统可以是设备、挂载点、卷标、`UUID=...`
* 无法理解的选项自动传递给特定的检查程序
## mdadm 软RAID管理
### 创建并启用
```sh
mdadm -C /dev/md? -l 等级 -n raid块数 [-x 冗余块数] 块设备或分区1 ...
```
* 设备数必须等于`-n`和`-x`参数之和
* 命令返回后RAID可能仍在重建
### 详情
```sh
mdadm -D /dev/md?
```
### 管理
```sh
mdadm /dev/md? -a|-f|-r 块设备或分区1 [...]
```
选项|意义
-|-
-a|添加空闲设备
-f|标记为出错，若有空闲设备则自动重建
-r|删除不活跃设备
### 停止
```sh
mdadm -S /dev/md?
```
### 启用
```sh
mdadm -A /dev/md? -u 块设备或分区共同UUID|块设备或分区1 ...
```
## mke2fs 格式化ext分区
```sh
mke2fs [选项] 分区
```
选项|意义
-|-
-b 字节数|指定块大小，可选1024、2048、4096
-c|在格式化前检查坏块（指定`-c -c`做读写测试，否则只读测试）
-i 字节数|指定i节点数（为多少字节预留一个i节点），不应小于块大小
-j|加入日志（ext3）
-L|指定卷标
## mkfs 格式化分区
```sh
mkfs[.文件系统] 分区
```
默认ext2
## mkisofs 制作光盘镜像
```sh
mkisofs [选项] -o 镜像 文件1 [...]
```
选项|意义
-|-
-m glob|排除glob匹配的文件，可以是文件名或路径
-r|不限制文件名8字符.3扩展名
-v|输出详情
-V 卷标|指定卷标
--graft-points|允许以`位置=文件`参数指定文件在光盘中的位置，否则将文件参数和目录参数下的文件置于光盘根目录
## mkswap 格式化swap分区
```sh
mkswap [选项] 分区
```
选项|意义
-|-
-L 卷标|指定卷标
-U UUID|指定UUID
## mount 挂载分区
```sh
mount [选项] [分区] [目录]
```
选项|意义
-|-
无|（仅向后兼容）列出挂载的分区
-a|按`/etc/fstab`中的顺序挂载尚未挂载的分区
-B|绑定挂载
-l|列出分区时显示标签
-n|挂载时不写入`/etc/mtab`（当`/etc`只读时使用）
-o 选项|挂载选项，`,`分隔
-t 文件系统|指定文件系统
-v|详细模式
* 绑定挂载：将文件系统的子目录挂载到另一目录
* 分区可用`-L 卷标`或`-U UUID`指定
* 若目录非空，旧内容被屏蔽，卸载后可见
* 不指定文件系统，则使用blkid库猜测，若无法得出结论，则依次测试`/etc/filesystems`下的类型（不带`nodev`的，下同），若该文件不存在或以一个`*`结尾，则再测试`/proc/filesystems`；测试时使用`slient`挂载选项

挂载选项|意义
-|-
async\|sync|异步/同步I/O
atime\|noatime|读取时由内核决定（更新与否）/不更新Access时间，默认atime
auto\|noauto|可/不可被`mount -a`挂载
bind|绑定挂载
defaults|rw,suid,dev,exec,auto,nouser,async（实际的默认选项由内核和文件系统类型决定）
dev\|nodev|解释/不解释此文件系统中的特殊设备文件
exec\|noexec|允许/禁止执行二进制文件
loop\[=loop设备\]\[,offset=偏移字节\]|显式指定通过loop设备挂载
noquota|禁用配额限制
relatime|仅当Access不晚于Modify或Change才更新，默认
remount|重新挂载已挂载的分区，只需指定分区、目录之一
ro\|rw|只读/可写分区
suid\|nosuid|启用/禁用SUID和SGID功能
user\|nouser|可/不可由普通用户挂载
usrquota\|grpquota|启用用户/组配额限制
* 非`remount`时不指定分区或目录之一，则按`/etc/fstab`中的条目挂载
* 对于绑定挂载，`ro`只适用于新目录
* `user`导致`noexec`、`nosuid`和`nodev`，除非在其后开启相反选项
## parted 磁盘分区
### 查看所有块设备的分区
```sh
parted -l
```
### 修改分区
```sh
parted 设备 [命令 [参数]]
```
命令|意义
-|-
mklabel 标签|创建磁盘标签，标签可为gpt、msdos等
mkpart [分区类型\|分区名称 文件系统类型] 起点 终点|新建分区，对于msdos需指定分区类型为primary、extended或logical，对于gpt需指定分区名称
print|输出分区表
rm 分区号|删除分区
* 起点、终点为MB数
## partprobe 通知内核磁盘分区表更改
```sh
partprobe
```
## resize2fs 调整ext文件系统大小
```sh
resize2fs 分区 [大小]
```
* 大小默认为分区全部可用空间
* 挂载状态不能缩小
## restore 恢复
```sh
restore [动作] -f 备份文件
```
动作|意义
-|-
-C|查看备份文件中的文件的更改情况
-i|交互模式，恢复部分文件
-t|列出备份文件中的文件
-r|恢复
* `-r`：工作目录需为文件系统根目录，对空文件系统可恢复等级0的备份，然后可依次恢复高级备份

交互命令|意义
-|-
add|添加到提取列表
cd|修改目录
delete|从提取列表删除
extract|提取到当前目录，volume选1，修改`.`权限选n
ls|列出文件，文件名前缀`*`表示已添加到解压列表
help|列出命令菜单
## swapon 启用swap分区
```sh
swapon [分区]
```
* 无分区则列出当前启用的swap分区
## swapoff 停用swap分区
```sh
swapoff 分区
```
## tune2fs 调整ext文件系统
```sh
tune2fs [选项] 分区
```
选项|意义
-|-
-j|加入日志（ext2转ext3）
-l|显示超级块信息
-L 卷标|设置卷标
### ext3转ext4
```sh
tune2fs -O dir_index,uninit_bg 分区
```
### ext2转ext4
```sh
tune2fs -O dir_index,uninit_bg,has_journal 分区
```
## umount 卸载分区
```sh
umount [选项] 分区或目录
```
选项|意义
-|-
-f|强制卸载（无法读取的NFS）
-n|不写入`/etc/mtab`
