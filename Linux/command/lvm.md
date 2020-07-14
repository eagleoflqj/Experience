# 逻辑卷管理
## lvcreate 新建逻辑卷
```sh
lvcreate 容量 [-n 逻辑卷] 卷组
lvcreate -s 容量 [-n 快照逻辑卷] [/dev/]卷组/逻辑卷
```
* 容量为`-l PE数`或`-L 大小`
* 初始内容最多存一次

逻辑卷|快照内容|快照实际存储
-|-|-
abc|abc|-（初始）
ac|abc|b（写时复制）
ac|abd|bd
## lvdisplay 显示逻辑卷属性
```sh
lvdisplay [[/dev/]卷组/逻辑卷1 ...]
```
## lvremove 删除逻辑卷
```sh
lvremove [/dev/]卷组/逻辑卷
```
## lvresize 调整逻辑卷大小
```sh
lvresize [-r] 容量 [/dev/]卷组/逻辑卷
```
* `-r`：同时改变逻辑卷内文件系统大小
* 容量为`-l [+|-]PE数`或`-L [+|-]大小`
## lvscan 列出逻辑卷
```sh
lvscan
```
## pvcreate 新建物理卷
```sh
pvcreate 分区1 [...]
```
## pvdisplay 显示物理卷属性
```sh
pvdisplay [PV分区1 ...]
```
## pvmove 在物理卷间移动PE块
```sh
pvmove 源PV分区 目的PV分区
```
## pvremove 删除物理卷
```sh
pvremove PV分区1 [...]
```
## pvscan 列出物理卷
```sh
pvscan
```
## vgcreate 新建卷组
```sh
vgcreate [-s PE大小] 卷组 PV分区1 [...]
```
* PE大小默认4M，必须为所有PV分区的最大扇区大小的2的幂次倍
* VG最多支持65534个PE
## vgdisplay 显示卷组属性
```sh
vgdisplay [卷组1 ...]
```
## vgextend 向卷组加入物理卷
```sh
vgextend 卷组 PV分区1 [...]
```
## vgreduce 从卷组移除物理卷
```sh
vgreduce 卷组 PV分区1 [...]
```
## vgremove 删除卷组
```sh
vgremove 卷组1 [...]
```
## vgscan 列出卷组
```sh
vgscan
```
