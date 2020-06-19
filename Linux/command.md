# 终端
## apropos 手册中关键字
```sh
apropos 名称
```
* 相当于`man -k`
## bc 计算器
```sh
bc [选项]
```
选项|意义
-|-
-l|使用标准数学库，并令scale=20，否则scale=0

命令|意义
-|-
scale=位数|指定小数精度
quit|退出
## cal 日历
```sh
cal [[月] 年]
```
## clear 清屏
```sh
clear
```
## cowsay 奶牛说话
```sh
cowsay 文字
```
## dc 逆波兰计算器
```sh
dc
```
命令|意义
-|-
f|输出栈
n|弹出并输出栈顶，不换行
p|输出栈顶
P|弹出并输出栈顶
## env 在指定环境执行命令
```sh
env [选项] [变量1=值1 ...] [命令 [参数1 ...]]
```
选项|意义
-|-
-i|清空环境
-u 变量|删除变量

* 命令前的变量赋值相当于export
* 不指定命令则输出选项所指定的环境
* env总是位于`/usr/bin`，可用于shebang中指定当前环境中的解释器
## expr 表达式求值
```sh
expr 表达式
```
* 运算数和运算符应为不同参数
## info 查看Info文档
```sh
info 名称
```
标题|意义
-|-
File|当前页面的文件，非info格式则页面内容同man
Node|当前节点
Next|后一节点，按`n`进入
Prev|前一节点，按`p`进入
Up|父节点，按`u`进入
## locale 显示本地化信息
```sh
locale [选项]
```
选项|意义
-|-
无|当前本地化设置
-a|可选的本地化
## login 登录账户
```sh
login 用户名
```
## man 查看手册
```sh
man [选项] 名称
```
选项|意义
-|-
-f|更多相关信息
-k|关键字查找
数字|指定手册条目

数字|意义
-|-
1|用户在shell环境中可以操作的命令或可执行文件
2|系统内核可调用的函数与工具等
3|一些常用的函数与库，大部分为C函数库libc
4|设备文件的说明，通常在/dev下
5|配置文件或某些文件的格式
6|游戏
7|惯例与协议等，例如Linux文件系统、网络协议、ASCII code等说明
8|系统管理员可用的管理命令
9|跟kernel有关的文件

标题|意义
-|-
NAME|简短的命令、数据名称说明
SYNOPSIS|简短的命令执行语法简介
DESCRIPTION|较完整的说明
OPTIONS|针对SYNOPSIS中所有可用的选项说明
COMMANDS|当程序执行时可在其内部执行的命令
ENVIRONMENT|相关环境变量
FILES|程序使用、参考或连接到的文件
SEE ALSO|其他说明
EXAMPLE|参考范例
BUGS|相关错误
## printenv 输出环境变量
```sh
printenv [变量]
```
* 给定变量输出值，否则输出所有`变量=值`
## reset 重置终端
```sh
reset
```
* 恢复由于`cat`二进制文件导致的终端乱码
## sleep 暂停一段时间
```sh
sleep 时间[后缀]
```
* 时间可以是小数，后缀可以是smhd，无后缀为s
## startx 启动X Window
```sh
startx
```
## stty 终端设置
```sh
stty 设置项 按键
stty -a
```
* `-a`：列出全部设置
## su 切换用户
```sh
su [选项] [用户]
```
选项|意义
-|-
-|启动登录shell
-s 登录shell|指定登录shell

* 用户默认root
## tty 输出连接到标准输入的终端文件
```sh
tty
```
## whatis 手册中相关信息
```sh
whatis 名称
```
* 等价于`man -f`
## xargs 切分标准输入作为参数
```sh
xargs [选项] [命令 [命令的前几个参数]]
```
选项|意义
-|-
-0|`\0`分隔，默认空白字符分隔
（不推荐）-e字符串|同`-E`，但`-e`必须紧跟字符串
-E 字符串|切分出该字符串则终止，不传递该字符串
-n 个数|每次执行命令使用几个参数
-p|询问是否执行
* 命令默认`/bin/echo`
# 文本
## cat 输出内容
```sh
cat [选项] [文件1 ...]
```
选项|意义
-|-
-A|等价于-vET
-E|行尾追加$
-n|显示行号
-T|\t用^I代替
-v|显示特殊字符，如\r用^M代替
* 文件默认标准输入
### 合并split生成的文件
```sh
cat 前缀* >文件
```
## cmp 逐字节比较文件
```sh
cmp [-l] 文件1 文件2
```
* `-l`：输出全部字节差异，否则只输出第一处
## col ASCII行处理工具
```sh
col [选项]
```
选项|意义
-|-
-b|`\b`不保留被取代的字符
-x|`\t`转对应数量的空格
* 从标准输入读取，写入标准输出
## cut 行剪切
```sh
cut [-b|-c] [文件1 ...]
cut [-f -d] [文件1 ...]
```
选项|意义
-|-
-b 范围|指定字节范围（`1-2,3,4-`形式，下同），从1开始
-c 范围|指定字符范围，从1开始
-d 分隔符|指定分隔符
-f 范围|指定切分出的部分，从1开始，多个部分用分隔符连接后输出
* 文件中连续的分隔符将切分出空串
## dd 字节复制
```sh
dd [参数]
```
参数|意义
-|-
if=文件|指定输入文件，否则为stdin
of=文件|指定输出文件，否则为stdout
bs=大小|指定一次读取写入量（K同KiB，KB为十进制），否则为512字节
count=个数|指定复制的块个数
conv=选项|指定转换方式，`,`分隔
* 输出的`a+b`表示`a`个完整和`b`个不完整`bs`

选项|意义
-|-
notrunc|不截短已有的输出文件
### 制作启动盘
```sh
dd if=iso文件 of=/dev/sd? bs=4M
```
## diff 比较文件差异
```sh
diff [选项] 参数1 参数2
```
选项|意义
-|-
-a|二进制文件按文本处理
-b|忽略连续空白字符数差异
-B|忽略空白行
-i|忽略大小写
-N|视不存在的文件为空文件（允许patch新建/删除）
-r|递归子目录
-u|输出3行上下文
* 若一个参数是文件，另一个参数可为`-`（标准输入）
* 若参数为一个文件和一个目录，则比较目录下同名文件
* 若两个参数均为目录，则比较目录下的文件名和同名文件
## dos2unix/unix2dos CRLF/LF互转
```sh
dos2unix/unix2dos [选项] 文件 [...]
```
选项|意义
-|-
-k|保留时间戳
-n|保留文件，文件后的参数为新文件
## expand `\t`转空格
```sh
expand [-t 数字] [文件]
```
* `-t`指定分组长度，默认8
* 写入标准输出，文件不存在或`-`为标准输入
## file 查看文件类型
```sh
file [选项] 文件
```
选项|意义
-|-
-i|输出MIME类型
## grep 查找符合条件的行
```sh
grep [选项] 正则表达式 文件
```
选项|意义
-|-
-a|二进制文件按文本处理
-A\|B 行数|同时输出匹配行后/前的多行，多个结果间插入`--`行
-c|只显示匹配行数
-E|扩展正则表达式，同`egrep`
-i|忽略大小写
-l|只显示文件名
-n|显示行号
-r|递归查找
-v|选择不匹配的行
--color=always\|auto\|never|控制颜色
* GNU grep的基础和扩展正则表达式表达能力相同
* 基础正则表达式的`?`、`+`、`{`、`|`、`(`、`)`是普通字符，作为特殊字符需要用`\`转义
* 扩展正则表达式上述字符是特殊字符，作为普通字符需要用`\`转义
## hd 查看字节及ASCII
```sh
hd [选项] 文件
```
选项|意义
-|-
-n 字节数|显示指定字节数
-s 字节数|跳过指定字节数
## head 查看文件头部
```sh
head [选项] 文件
```
选项|意义
-|-
-c 字节数|显示前几字节
-c -字节数|不显示后几字节
-行数或-n 行数|显示前几行，默认10
-n -行数|不显示最后几行
## iconv 编码转换
```sh
iconv [选项] [文件]
iconv -l
```
选项|意义
-|-
-f 编码|指定输入编码，默认当前locale
-t 编码|指定输出编码，默认当前locale
-o 文件|指定输出文件，默认标准输出
-l|列出支持的编码
* 文件默认标准输入
### UTF-8下简体转繁体
```sh
iconv -t gb2312 简体文件 | iconv -f gb2312 -t big5 | iconv -f big5 -o 繁体文件
```
* GB2312只含简体字，BIG-5只含繁体字，可以对应转换
* GBK兼容GB2312，繁体字部分和BIG-5对应，简体字无法转换为BIG-5
## join 关系型内连接
```sh
join [选项] 文件1 [文件2]
```
选项|意义
-|-
-1或-2 数字|指定第1/2个文件用第几个字段，从1开始
-i|忽略大小写
-t 分隔符|指定分隔符

* 默认以空白字符串分隔，第1个字段相同则连接，要求字段排序
* 相同的公共字段为结果第一个字段，其他字段按在文件中出现的顺序列出
* 写入标准输出，文件2不存在则为标准输入
## less 高级翻页查看
```sh
less [选项] 文件1 [... 文件n]
```
选项|意义
-|-
-m|显示百分比
-N|显示行号

按键|意义
-|-
b|后退1屏
f|前进1屏
Enter或j|前进1行
k|后退1行
g|到第1行
G|到最后1行
/字符串|向前查找字符串
?字符串|向后查找字符串
n|下一个
N|上一个
:p|切换到下一个文件
:n|切换到上一个文件
:x|切换到第1个文件
:d|从列表移除当前文件
## md5sum 检查md5
```sh
md5sum [选项] [文件]
```
选项|意义
-|-
-c 文件|从文件中读取md5和文件名并校验

* `-c`指定的文件每行为`md5 文件名`
## more 翻页查看
```sh
more [选项] 文件
```
选项|意义
-|-
+行数|从第几行开始显示
+/字符串|从第一次匹配到字符串的前2行开始显示

按键|意义
-|-
Enter|前进1行
Space|前进1屏
b|后退1屏
=|输出当前行号
:f|输出文件名和当前行号
q|退出
## nano 编辑文件
```sh
nano [文件]
```
快捷键|意义
-|-
Ctrl+C|显示当前行列
Ctrl+O|保存
Ctrl+R|打开
Ctrl+W|查找
Ctrl+X|退出
## nl 带行号显示文件
```sh
nl [选项] 文件
```
选项|意义
-|-
-ba|空行也编号，默认不编号
-nln|行号左对齐，默认右对齐
-nrz|行号右对齐，左补0
-w 位数|行号占用最小位数，默认6
## od 查看字节
```sh
od [选项] [... 文件n]
```
选项|意义
-|-
-A 进制符|指定偏移量进制
-j 字节数|跳过指定字节数
-N 字节数|显示指定字节数
-t a|ASCII
-t c|可打印字符或\转义
-t 进制符\[组字节数\]|指定进制和每组字节数
-v|详细模式，重复行不用*代替
--endian=big或little|指定每组多字节时的大小端

传统选项|意义
-|-
-d|同`-t u2`
-o|同`-t o2`，默认
-x|同`-t o2`

未指定文件，则使用标准输入，Ctrl+D结尾
## paste 逐行拼接
```sh
paste [-d 分隔符] [文件1 ...]
```
* 分隔符默认`\t`
* `-`表示标准输入
## patch 用diff修改文件
```sh
diff -Naur 文件1 文件2 >补丁文件 # 补丁=文件2-文件1
patch [选项] [文件 [补丁文件]] # 文件+=补丁
```
选项|意义
-|-
-b|备份原文件为`文件1.orig`
-i 补丁文件|指定补丁文件，否则为stdin
-N|禁用`-R`，否则询问
-p 个数|删去补丁文件中文件名路径的前缀数
-R|当patch失败时假定补丁搞错了新旧文件（文件-=补丁），否则询问
* 不指定补丁文件则从标准输入读取
* 不指定文件则从补丁文件提取文件1
* 失败的补丁保存为`原文件.rej`
## pr 转换文本为打印格式
```sh
pr 文件
```
* 加入文件Modify时间、文件名、页码
* 写入标准输出，文件不存在或`-`为标准输入
## sort 排序输出内容
```sh
sort [选项] 文件1 [... 文件n]
```
选项|意义
-|-
-b|忽略行首空格
-f|忽略大小写
-k 数字|配合`-t`，指定排序依据，从1开始
-M|其他&lt;JAN<...&lt;DEC
-n|数值序，默认字典序
-r|降序，否则升序
-t 分隔符|指定分隔符，默认空白字符
-u|去重
## split 文件切分
```sh
split [选项] [文件 [前缀]]
```
选项|意义
-|-
-b 大小|指定分块大小
-l 行数|指定分块行数，默认1000
* 文件不存在或`-`为标准输入
* 前缀默认x，后面添加字母，写入当前目录
## tac 倒序输出内容
```sh
tac 文件
```
## tail 查看文件末尾
```sh
tail [选项] 文件
```
参数|意义
-|-
-f|实时监视
+行号或-n +行号|从某行开始显示
-行数或-n 行数|显示最后几行，默认10
## tee 读标准输入，写标准输出和文件
```sh
tee [选项] [文件1 ...]
```
选项|意义
-|-
-a|追加，否则覆盖
## tr 字符串转换
```sh
tr 集合1 集合2
tr -d 集合
tr -s 集合1 [集合2]
```
选项|意义
-|-
-d|删除字符
-s|合并重复字符
* 集合2为对应的替换字符集
* 从标准输入读取，写入标准输出
## truncate 调整文件大小
```sh
truncate [参数] [-s 大小 文件]
```
* 原文件较大则删去结尾，较小则补空字符，不存在则新建
* 大小默认字节数，k、m为1024幂，KB、MB为1000幂

参数|意义
-|-
-c|不新建文件
--help|显示帮助
## uniq 连续行去重
```sh
uniq [选项] [输入 [输出]]
```
选项|意义
-|-
-c|统计重复次数
-i|忽略大小写
* 输入/输出默认标准输入/输出
## wc 字数统计
```sh
wc [选项] [文件]
```
选项|意义
-|-
-c|字节数
-m|字符数，若不支持多字节字符则同`-c`
-l|换行符数
-L|最长行显示宽度
-w|空白字符分开的单词数
* 文件不指定则为stdin
* 默认-lwc
# 压缩
## bzcat/zcat 查看bzip2/gzip压缩的文本文件
```sh
bzcat/zcat 文件
```
## bzip2/gzip
```sh
bzip2/gzip [选项] 文件
```
选项|意义
-|-
无|压缩并替换，增加.bz2/.gz后缀
-c|不替换文件，输出压缩结果到标准输出
-d|解压并替换，删除.bz2.gz后缀
-t|检查压缩文件完整性
-v|显示压缩比
-数字|-1最快，-9最小，默认-6
# 角色
## chgrp 设置所属组
```sh
chgrp [选项] 组名 文件
```
选项|意义
-|-
-R|递归设置
## chown 设置所属用户
```sh
chown [选项] 用户名[:组名] 文件
```
选项|意义
-|-
-R|递归设置
## groupadd 添加用户组
```sh
groupadd 组名
```
## groupdel 删除用户组
```sh
groupdel [-f] 组名
```
-f强制删除，组内用户所属组只有组编号
## groupmod 修改用户组
```sh
groupmod -n 新组名 组名
```
## groups 查看用户所属用户组
```sh
groups [用户名]
```
默认当前用户  
输出用户名:用户组
## id 输出用户和组id
```sh
id [选项] [用户名或id]
```
选项|意义
-|-
-g|只输出主组id
-G|输出所有组id
-u|只输出用户id
## passwd 设置密码
```sh
passwd [选项] [用户名]
```
* 默认为当前用户

选项|意义
-|-
无|设置新密码
-d|删除密码（隐含取消锁定状态）
-l|锁定（禁用密码，在`/etc/shadow`密码项前添加`!`）
-u|解锁（恢复密码）
## useradd 添加用户
```sh
useradd [选项] 用户名
```
选项|意义
-|-
-g 组名|指定组名，否则与用户同名
-k 框架目录|指定`-m`时的框架目录，否则由`/etc/default/useradd`中`SKEL`变量指定，或`/etc/skel`
-m|自动建立用户目录/home/用户名
-s 登录shell|指定登录shell，否则由`/etc/default/useradd`中`SHELL`变量指定（`/bin/bash`）
* 若同名组已存在则必须指定组名
* 禁止登录shell是`/sbin/nologin`
## userdel 删除用户
```sh
userdel [选项] 用户名
```
选项|意义
-|-
-f|强制删除
-r|同时删除用户目录、mail spool

* 被删用户已登录时，可以强制删除，但不会自动登出，新建的文件没有用户名只有用户编号
* 被删用户所属文件的所有者变为编号，若新增用户的编号等于此编号，则继承此文件
* 被删用户若与所属用户组同名且是组里唯一用户，会同时删除用户组
## usermod 修改用户
```sh
usermod [选项] 用户名
```
选项|意义
-|-
-aG 组名|添加组
-g 主组|修改主组
-l 用户名|修改用户名
## visudo 编辑sudo配置文件
```sh
visudo
```
### 开启sudo日志
```
Defaults logfile=/var/log/sudo
```
### 设置sudo用户组
```
%sudo ALL=(ALL) ALL
```
第一个ALL为所有主机，第二个为所有用户，第三个为所有命令
# 权限
## 文件权限
### r 读取文件
包括用解释器执行文件
### w 写入文件
### x 执行文件
./形式执行
### s SUID/SGID
* 仅对二进制程序有效
* 执行者需有x权限，执行中临时获得所属者/组的权限
* 若u/g不具有x权限，则`ls -l`的ux/gx位显示为S
## 目录权限
### r 列出目录下的文件
### w 修改目录
* 新建文件、重命名文件、删除文件（无论文件所有者与权限）
* 需要x权限
### x 进入目录
* 读取目录下的文件必须具有目录的x权限，可以无目录的r权限
### s SGID
* 使用者需有rx权限，进入目录后临时获得所属组的权限
* 若使用者有w权限，则在此目录下新建的文件所属组为该组
### t 粘滞位SBIT
* `chmod o+t`后，目录下文件所有者以外的用户即使拥有目录w权限，也无法删除文件
* 若o不具有x权限，则`ls -l`的ox位显示T
## aa-complain 设置程序为AppArmor抱怨模式
```sh
aa-complain 可执行文件
```
## aa-status 查看AppArmor状态
```sh
aa-status
```
## chmod 设置权限
普通用户只能设置自己的文件  
u所有者 g用户组 o其他人 a所有
r4 w2 x1  
=设置权限 +授予权限 -收回权限  
### 惯例
```sh
chmod 755 目录
chmod 644 文件
```
### 皆可读
```sh
chmod ugo+r 文件
chmod a+r 文件
```
### u可执行，g只读，o不可写
```sh
chmod u+x,g=r,o-w 文件
```
### 递归设置权限
```sh
chmod -R 644 * # 当前文件夹下的文件
chmod -R 644 . # 当前文件夹及其文件
# chmod -R 000 / # 禁令
```
## getfacl 查看细分权限
```sh
getfacl [选项] 文件
```
选项|意义
-|-
-R|递归输出
## setfacl 设置细分权限
```sh
setfacl [选项] 文件
```
选项|意义
-|-
-b|删除细分权限
-m ACL项|修改细分权限
-R|递归设置
--test|输出设置后的结果但不设置
### ACL项
* u:\[用户\]:权限
* g:\[用户组\]:权限
* o:权限
* 不指定用户（组）则默认所属用户（组）
### 权限
* rwx或0-7
# 目录
## basename 路径尾
```sh
basename 路径
```
## chattr 设置Linux文件系统中的属性
```sh
chattr [-R] +|-|=属性串 文件1 ...
```
属性|意义
-|-
s|删除后清零磁盘块
u|删除后保留内容
S|同步写入磁盘
i|不可修改、删除、移动或创建硬链接
a|只可追加
d|被dump程序忽略
A|读取文件时不更新访问时间，写入后A属性自动消失
c|写时压缩，读时解压
e|在逻辑块到物理块的映射中使用了extent，该属性可能无法修改
* ext文件系统中`c`、`s`、`u`尚未被当前（5.6.14）主线内核实现
* `a`、`i`不影响已打开的文件描述符
## cp 复制
```sh
cp [选项] 源 目的
cp [选项] 源1[... 源n] 目的目录
```
选项|意义
-|-
-a|等价于-dpr
-d|对符号链接，复制链接本身，否则最终指向
-f|强制，针对拥有目录写权限但无文件写权限的情况
-i|交互，文件已存在则询问是否覆盖
-l|硬链接
-p|在权限许可范围内保留属性
-r|递归复制
-s|符号链接，目的必须在当前目录下
-u|更新，仅当源较新或目的不存在时复制

源|目的|行为
-|-|-
文件/目录|目录|复制到目录下
文件/目录|目录下新位置|复制
文件|文件|覆盖
目录|文件|非法
## cpio 复制文件进/出归档
```sh
cpio -o [-B] [选项]
cpio -i [-du] [选项]
cpio -t [选项]
```
* `-o`从标准输入读取文件列表（对目录不递归），归档至标准输出
* `-i`从标准输入读取归档，在当前目录提取
* `-t`从标准输入读取归档，列出归档内文件

选项|意义
-|-
-B|以5120字节为单位，否则512字节
-c|同`-H newc`
-d|按需建立目录（适用于归档了`d/f`但未归档`d`，当前目录下无`d`）
-H 格式|指定格式，默认`-i`时`bin`，`-o`或`-t`时自动检测
-u|较旧文件覆盖较新同名文件，否则不提取该文件
-v|输出处理的文件名，遇`-t`则分行输出详细信息
## dirname 路径去尾
```sh
dirname 路径
```
## du 查看目录大小
```sh
du [选项] [目录]
```
选项|意义
-|-
无|同-k，除非设置了`$POSIXLY_CORRECT`（默认以512字节块为单位）
-a|同时列出文件，否则只有目录
[-d \|--max-depth=]层数|统计的子目录层数
-h|人类可读*iB单位
-k|以KiB为单位
-m|以MiB为单位
-s|同-d 0
-S|对目录显示其自身大小，而不包括子目录大小
## find 查找文件
```sh
find [选项] [起点1 ...] [表达式]
```
选项|意义
-|-
-L|检查符号链接最终指向的文件，否则链接本身

* 起点默认`.`

表达式是如下几类的组合
* 测试：基于文件性质返回true/false
* 动作：执行动作，基于成功与否返回true/false
* 全局选项：决定测试和动作的方式，返回true
* 位置选项：影响其后的表达式，返回true
* 运算符：连接各表达式

位置选项|意义
-|-
-daystart|从当天0点起算，否则从24小时前算

测试|意义
-|-
-amin\|cmin\|mmin 分钟数|上次Access/Change/Modify时间
-anewer\|cnewer\|newer 文件|在给定文件被修改后A/C/M过
-atime\|ctime\|mtime 天数|上次A/C/M的24小时数
-empty|空文件或空目录
-fstype 文件系统|指定文件所属文件系统
-gid 组号\|-group 组|指定所属组
-iname 文件|忽略大小写的`-name`
-inum inode号|指定inode号
-iregex 正则表达式|忽略大小写的`-regex`
-links 硬链接数|指定文件硬链接数
-lname 文件|直接指向的文件名，`-L`时直接指向的文件不存在时返回true
-name 文件|查找文件名，不含前导目录，不受`-L`影响
-newerXY 参照|X时间在参照的Y时间之后
-nogroup\|nouser|所属组/用户号没有组/用户名
-perm 权限|精确等于权限，3/4位八进制或`,`分隔`ugo=`
-perm -权限|包含所有权限
-perm /权限|包含某一权限
-readable\|writable\|executable|可读/写/执行
-regex 正则表达式|完整路径匹配正则表达式
-samefile 文件|和给定文件inode相同
-size 个数\[单位\]|指定大小，单位默认`b`
-type 类型|指定类型，`f`为普通文件，其余同`ls -l`
-used 天数|A比C晚的天数
-uid 用户号\|-user 用户|指定所属用户

* 时间+n、n、-n分别为大于n+1、介于n和n+1、小于n
* 大小+n、n、-n分别为大于、等于、小于
* X/Y可为a、B、c、m，Y为t表示参照直接当作时间解释

单位|意义
-|-
b|512字节块
c|字节
w|双字节
k|KiB
M|MiB
G|GiB

动作|意义
-|-
-delete|删除，隐含`-d`
-exec 命令 \\;|执行命令，以{}指代匹配的文件
-print|按行输出完整路径，默认行为
-print0|输出完整路径，`\0`分隔

全局选项|意义
-|-
-d|后根遍历，否则先根
-maxdepth\|-mindepth 层数|指定最深/最浅层数，否则不限/0
-mount|不进入其他文件系统

## ln 创建指向目标的链接
```sh
ln [选项] 目标 链接
```
选项|意义
-|-
-f|强制，覆盖已存在的文件
-i|交互，文件已存在则询问是否覆盖
-n|若链接已存在且指向目录，试其为普通文件，操作将覆盖或失败
-s|符号链接，默认硬链接

* 若链接为目录，则在该目录下创建与目标末端同名的链接
* 不可硬链接到目录或另一分区的文件
## locate 在数据库查找文件
```sh
locate [选项] 模式
```
选项|意义
-|-
-i|不区分大小写
-r|正则表达式，否则globbing
* globbing模式下如果无通配符，等价于`*模式*`
## ls 列出文件
```sh
ls [选项] [目录或文件]
```
选项|意义
-|-
-a|包括隐藏的文件
-A|同-a，但不包括`.`和`..`
-ct|按修改时间降序
-d|只列出目录本身，不列出其下文件
-F|文件名追加尾缀
-i|列出i节点号
-l|分行显示详细信息
-n|同-l，但用户和组使用UID和GID
-r|反序
-s|占用空间，以块为单位
-R|递归输出
-S|按文件大小降序
-X|按扩展名升序
--block-size=块大小|指定块大小
--color=always\|auto\|never|控制颜色
--full-time|显示完整时间
--time=atime\|ctime|时间显示为Access/Change，否则Modify

尾缀|意义
-|-
/|目录
*|可执行文件
@|符号链接
=|套接字
\||命名管道

类型|意义
-|-
-|普通文件
b|块设备
c|字符设备
d|目录
l|链接
p|命名管道
s|套接字
* 详情第二列为i节点硬链接数，即指向该i节点的目录项数
* 默认`-ls`显示的大小为字节，占用空间为1KiB块，total为总占用块数
* 块大小为数字和可选单位，K同KiB，KB为十进制
## lsattr 查看Linux文件系统中的属性
```sh
lsattr [选项] [文件]
```
选项|意义
-|-
-a|包括隐藏的文件
-d|只列出目录本身，不列出其下文件
-l|显示属性长名字，否则字母
-R|递归
* 文件默认.
## mkdir 创建目录
```sh
mkdir [选项] [目录]
```
选项|意义
-|-
-m 权限|指定权限
-p|自动创建不存在的父目录
-v|对每个创建的目录输出一条信息
* 新建目录硬链接数为2（父目录的目录项和该目录的`.`），其父目录硬链接数+1（该目录的`..`）
## mknod 创建特殊文件
```sh
mknod [选项] 文件 类型 [主设备号 次设备号]
```
选项|意义
-|-
-m 权限|设置权限
* 类型为`b`、`c`或`u`必须指定主次设备号
* 类型为`p`不指定主次设备号
* 主设备号决定驱动，驱动用次设备号区分设备
## mktemp 生成临时文件并输出文件名
```sh
mktemp [选项] [模板]
```
选项|意义
-|-
-d|创建目录，否则文件
-p 目录|相对于该目录解释模板
--tmpdir\[=目录]|同-p，目录默认`$TMPDIR`，未设置则`/tmp`
* 模板至少以`XXX`结尾，不指定则默认`tmp.XXXXXXXXXX`并导致`--tmpdir`
* 目录和文件的权限分别为700、600去掉umask
## mv 移动或重命名
```sh
mv [选项] 文件 新文件
mv [选项] 源1[... 源n] 目的目录
```
选项|意义
-|-
-f|强制
-i|交互
-u|更新
## readlink 输出链接指向
```sh
readlink [选项] 链接
```
选项|意义
-|-
-f|绝对路径，最终指向
## realpath 输出绝对路径
```sh
realpath 路径
```
## rm 删除
```sh
rm [选项] 文件或目录
```
选项|意义
-|-
-f|强制删除
-i|交互
-r|递归删除
## rmdir 删除空目录
```sh
rmdir [选项] 目录
```
选项|意义
-|-
-p|删除子目录后，依次试图删除路径中的父目录
-v|对每个试图删除的目录输出一条信息
## scp 远程复制
```sh
scp [选项] [前缀]源 [前缀]目的
```
选项|意义
-|-
-r|递归复制

前缀：用户名@ip地址:
## stat 文件或文件系统状态
```sh
stat [选项] 文件
```
选项|意义
-|-
-f|显示文件系统状态，默认文件状态

标题|意义
-|-
Size|字节数
Blocks|512字节块数
IO Block|文件系统块字节数
Device|文件系统所在分区的主次设备号（hex和decimal）
Links|硬链接数
Access|读取时间
Modify|内容修改时间
Change|元信息修改时间
Birth|创建时间
## tar 归档处理
```sh
tar 动作 [选项] -f 归档文件 [被归档文件 ...|待解包文件 ...]
```
动作|意义
-|-
-c|打包
-t|列出包内文件
-x|解包

选项|意义
-|-
-C 目录|解包到目录
-j|使用bzip2
-P|保留被归档文件路径，否则去掉`/`、父目录前缀
-v|输出细节
-z|使用gzip
--exclude=文件|不打包文件
--newer=日期或文件|Modify或Change不早于日期或文件的Modify，文件必须以`.`或`/`开头，下同
--newer-mtime=日期或文件|Modify不早于日期或文件的Modify
* 归档文件`-`为标准输入/输出
## tree 查看目录树
```sh
tree [参数] [目录]
```
参数|意义
-|-
-a|包括隐藏文件
-I 模式|排除项
## touch 修改文件时间
```sh
touch [选项] 文件
```
选项|意义
-|-
-a|只修改Acesss（和Change）
-c|不新建文件
-d 字符串|指定时间，字符串精确定义见info
-m|只修改Modify（和Change）
-t \[\[CC]YY]MMDDhhmm\[.ss]|指定时间
## whereis 命令相关文件
```sh
whereis [选项] 命令
```
选项|意义
-|-
-b|可执行文件
-m|手册
-s|源码
## which 命令程序位置
```sh
which [-a] 命令
```
* `-a`：列出`$PATH`中的所有命令，否则只显示第一个
# 进程
## free （虚拟）内存使用情况
```sh
free [参数]
```
参数|意义
-|-
-b|Bytes
-k|KiB，默认
-m|MiB
-g|GiB
-s 秒数|间隔刷新
## lsof 列出进程打开的文件
```sh
lsof [参数] [文件1 ...]
```
参数|意义
-|-
-a|AND逻辑，默认OR
-c 程序名前缀|指定程序名
-i \[4\|6]\[TCP\|UDP\]\[@主机\]\[:端口\][-s tcp:状态]|指定网络
-n|NAME列主机名显示为IP，默认域名
-P|NAME列端口显示为端口号，默认监听该端口常用应用名
-p 进程号|指定进程
-u 用户|指定用户
+c 整数|COMMAND列最长长度，小于len('COMMAND')=7时置为7
+d 目录|指定目录
+D 目录|指定递归目录
* 目录和文件是OR逻辑
## mono 执行.NET程序
```sh
mono exe程序
```
## nice 更改优先级执行
```sh
nice [-n 相对优先级] 命令
```
相对优先级-20~19，默认10，越低越优先，非管理员不得赋予负值
## nohup 忽略sighup执行
```sh
nohup 命令 [&]
```
* 遇到input则结束，输出和错误均定向至nohup.out  
* nohup和&同时使用时，关闭终端或exit当前shell，进程变为孤儿
## pidof 进程的PID
```sh
pidof 进程
```
## pkill 查找进程并发送信号
```sh
pkill [参数]
```
参数|意义
-|-
-F 文件|指定存放pid的文件
进程|指定进程
## ps 列出用户进程
```sh
ps [参数]
```
参数|意义
-|-
-e|所有进程
-f|全格式
-l|长格式
-H|按PPID树形结构
-o 小写列名1,...|指定列
-p &lt;PID 1&gt; ...|指定pid
--ppid &lt;PPID 1&gt;|指定ppid

* 行的指定是or关系

标题|意义
-|-
F|标志
S|状态
UID|用户ID
PID|进程号
PPID|父进程号
C|CPU使用率
PRI|调度优先级，数值越小越优先
NI|Nice值，对基准优先级的调整
ADDR|通常情况下，包含进程栈的段号；如果为内核进程，那么为预处理数据区的地址
SZ|占用内存KB
WCHAN|wait channel，正在因为哪个内核函数而休眠
STIME|启动时间
TTY|用户终端位置
TIME|CPU时间
CMD|启动命令

标志|意义
-|-
1|forked but didn't exec
4|used super-user privileges

状态|意义
-|-
D|Uninterruptible sleep (usually IO)
R|Running or runnable (on run queue)
S|Interruptible sleep (waiting for an event to complete)
T|Stopped, either by a job control signal or because it is being traced.
W|paging (not valid since the 2.6.xx kernel)
X|dead (should never be seen)
Z|僵尸，已终止但因父进程未调用wait，进程表仍保留其表项
* 前后台运行中未sleep：R
* 前后台运行中sleep、前台等待input：S
* 后台等待input、前台Ctrl+Z：T
* 子进程终止，自动向父进程发送SIGCHLD信号
* 父进程终止，子进程变为孤儿，被pid=1的init接管；init周期性调用wait，确保其子进程终止后不会长期处于僵尸状态
## pstree 显示进程树
```sh
pstree [选项] [进程号]
```
选项|意义
-|-
-A|用ASCII字符画树
-p|显示PID
## time 统计命令执行时间
```sh
time 命令
```
## top 任务管理器
```sh
top [参数]
```
参数|意义
-|-
-b|不接受input，滚动输出
-d 秒数|设置间隔
-n整数|迭代次数
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
* 文件系统可以是设备、挂载点、标签、`UUID=...`
* 无法理解的选项自动传递给特定的检查程序
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
relatime|仅当Access不晚于Modify或Change才更新，默认
remount|重新挂载已挂载的分区，只需指定分区、目录之一
ro\|rw|只读/可写分区
suid\|nosuid|启用/禁用SUID和SGID功能
user\|nouser|可/不可由普通用户挂载
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
# 系统
## crontab 定时任务
```sh
crontab [-u 用户名] 参数或文件
```
参数|意义
-|-
-l|列出定时任务
-e|用vi设定任务
-r|删除所有任务

```
分0-59 时0-23 日1-31 月1-12 周几0-7 命令
```
格式|意义
-|-
*|所有
数字[... ,数字]|枚举时间
数字-数字|时间段
*（或数字-数字）/数字|（指定时间段内）时间间隔

crontab的所有操作记录在/var/log/cron
## chroot 改变根目录执行命令/交互shell
```sh
chroot [选项] 新根目录 [命令 [参数]]
```
选项|意义
-|-
--skip-chdir|不切换工作目录到新根目录
## date 日期时间
```sh
date [选项] [+格式]
```
选项|意义
-|-
无|显示当前时间
-s yyyy-MM-dd hh:mm:ss|设置时间
-u|显示UTC时间
* 格式：`%Y-%m-%d\ %H:%M:%S`
## dmesg 查看/控制内核缓冲区
```sh
dmesg [选项]
```
选项|意义
-|-
无|输出
--read-clear|输出并清空
--clear|清空
## dmidecode 查看DMI信息
```sh
dmidecode [参数]
```
参数|意义
-|-
-t 数字或关键字|只显示给定类型设备

数字|意义
-|-
4|处理器
9|系统插槽
17|内存设备

关键字|数字
-|-
processor|4
slot|9
## halt/poweroff/reboot 结束所有进程/关机/重启
```sh
halt/poweroff/reboot [选项]
```
选项|意义
-|-
-f|强制

* `halt`不一定切断电源
## hdparm 查看/设置SATA/IDE硬盘参数
```sh
hdparm [选项] 硬盘
```
选项|意义
-|-
-i|查看启动时获得的硬盘信息
-I|查看当前硬盘信息
-t|测试实际性能
-T|测试缓存性能
## init 更改运行等级
```sh
init 等级
```
等级|意义
-|-
0|关机
1|单用户
2|多用户无网络，通常落入3
3|多用户
4|未定义
5|多用户+图形界面
6|重启
## insmod 装载内核模块
```sh
insmod 模块文件
```
## last/lastb 查看成功/失败登录
```sh
last/lastb [选项] [用户1 ...]
```
选项|意义
-|-
-n 行数|显示的行数
-s 时间|起始时间
-t 时间|终止时间
-p 时间|指定时间

时间|
-|
YYYYMMDDhhmmss|
\[YYYY-MM-DD\] \[hh:mm\[:ss\]\]|
now|
yesterday|
today|
tomorrow|
+5min|
-5days|
* 每次系统重启时记录伪用户reboot
## lastlog 查看用户最后登录时间
```sh
lastlog [选项]
```
选项|意义
-|-
-b 天数|晚于指定天数
-t 天数|早于指定天数
-u 用户|指定用户
## ldd 查看程序依赖库
```sh
ldd 程序
```
## lsb_release 查看LSB信息
```sh
lsb_release [选项]
```
选项|意义
-|-
无或-v|版本
-a|全部
## lsmod 列出装载的内核模块
```sh
lsmod
```
格式化输出/proc/modules
## lsusb 显示usb设备
```sh
lsusb
```
## lspci 显示PCI总线上设备
```sh
lspci [选项]
```
选项|意义
-|-
-s \[总线:\]\[设备\]\[.功能\]|过滤设备
-v\|-vv\|-vvv|显示详情
## rmmod 卸载内核模块
```sh
rmmod 模块
```
## runlevel 查看运行等级
```sh
runlevel
```
* 输出上一个运行等级和当前运行等级
* 为兼容SysV init，systemd将目标映射到运行等级；由于运行等级唯一而目标不唯一，映射只是近似的
* 无法确定的等级为`N`，两个均无法确定输出`unknown`

等级|目标
-|-
0|poweroff
1|rescue
2 3 4|multi-user
5|graphical
6|reboot
## service 服务启停
命令将被重定向至systemctl
```sh
service 服务 命令
```
命令|意义
-|-
start|启动
restart|重启
stop|停止
## shutdown 关机
```sh
shutdown [选项] [时间] [消息]
```
选项|意义
-|-
-c|取消
-h|关机
-r|重启

时间|意义
-|-
now|立即
数字|几分钟后
HH:MM|精确时刻
## strace 查看系统调用
```sh
strace 命令
strace -p 进程号
```
## sync 将内存中的修改写回硬盘
```sh
sync
```
## uname 查看系统信息
```sh
uname [参数]
```
参数|意义
-|-
-a|全部
-m|硬件平台
-r|内核版本
## updatedb 更新locate使用的数据库
```sh
updatedb
```
## update-rc.d 更新各运行等级的服务
```sh
update-rc.d 服务 命令
```
命令|意义
-|-
remove|清空`/etc/rc*.d`下的软链接
defaults|依照`/etc/init.d/服务`在`/etc/rc*.d`下添加软链接（需要先remove）
# 网络
## arp 操作ARP缓存
```sh
arp [选项] [模式]
```
选项|意义
-|-
-v|详细输出
-i 网卡|指定网卡
-t 类型|硬件类型，如ether

模式|意义
-|-
\[主机\]|查看缓存表
-d 主机|删除表项
-s 主机 MAC地址|添加表项
## curl 发送请求
```sh
curl [参数] URL
```
参数|意义
-|-
-d 字符串|指定body
-H 请求头|指定一行header
-X 方法|指定方法
--cacert 证书|指定CA证书
--data-binary @文件|指定body
## dhclient DHCP客户端
```sh
dhclient [网卡]
```
## dig DNS查询
```sh
dig [参数] [域名] [类型]
```
参数|意义
-|-
@服务器|指定dns服务器，否则/etc/resolv.conf中的地址，若无可用地址则尝试本地
-p 端口|指定端口，默认53
-4|只采用IPv4服务器
-6|只采用IPv6服务器
* 域名不存在则查询root
* 类型默认A
## etherwake 局域网唤醒
```sh
etherwake [选项] MAC地址
```
选项|意义
-|-
-b|以太网广播
-i 网卡|指定网卡，默认eth0
## ethtool 以太网卡工具
### 查看设置
```sh
ethtool 网卡
```
### 修改设置
```sh
ethtool -s 网卡 参数
```
参数|意义
-|-
wol 唤醒选项|设置Wake-on-Lan

唤醒选项|意义
-|-
g|魔包唤醒
d|禁用
## ifconfig 输出网络配置
```sh
ifconfig
```
## ip 网络配置
### 查看网卡及ip
```sh
ip address
```
### 添加IP
```sh
ip address add dev 网卡 local IP地址/掩码长度 peer IP地址/掩码长度
```
### 删除IP
```sh
ip address del IP地址[/掩码长度] dev 网卡
```
### 启停网卡
```sh
ip link set 网卡 up|down
```
### 查看邻居
```sh
ip neigh
```
## iperf3 测网速
通用选项|意义
-|-
-p 端口|指定端口，默认5201
### 服务器
```sh
iperf3 -s [选项]
```
选项|意义
-|-
-1|接受一个客户端连接后退出
### 客户端
```sh
iperf3 -c 主机 [选项]
```
选项|意义
-|-
-b 数字\[kmg\]|目标速率，0为无限；UDP默认1m，TCP默认0
-u|UDP，默认TCP
-R|下载，默认上传
## iwlist 查看无线网络
### 扫描无线网
```sh
iwlist [网卡] scan
```
## nc 传输层工具
```sh
nc [选项] 主机 端口
```
选项|意义
-|-
-4|只使用IPv4
-6|只使用IPv6
-l|服务端监听，默认客户端
-p 端口|指定客户端端口
-u|UDP，默认TCP
-v|输出详细信息
-w 秒数|客户端超时时间，默认无
-z|扫描端口
## nmcli NetworkManager客户端
### 连接wifi
```sh
nmcli device wifi connect 接入点 password 密码
```
### 查看连接
```sh
nmcli connection show
```
### 断开网络
```sh
nmcli connection down 连接UUID
```
### 重连网络
```sh
nmcli connection up 连接UUID
```
## ping ICMP测通
```sh
ping [参数] 主机
```
参数|意义
-|-
-c 次数|ping给定次，默认无限次
-i 秒数|指定请求间隔，默认1秒
-a|ping的同时响铃（需要终端支持）
## ss socket状态
```sh
ss [选项] [state 状态] [表达式]
```
选项|意义
-|-
-4|显示仅监听IPv4
-6|显示监听IPv6
-a|显示监听和连接
-i|显示TCP信息
-l|只显示监听，默认只显示连接
-n|端口显示为端口号，默认监听该端口常用应用名
-o|显示定时器
-p|显示进程
-r|主机名显示为域名，默认IP
-s|显示统计信息
-t|显示TCP
-u|显示UDP
-x|显示Unix域
-H|不显示标题
* 标准TCP状态：established、syn-sent、syn-recv、fin-wait-1、fin-wait-2、time-wait、closed、close-wait、last-ack、listening、closing

组合状态|意义
-|-
all|所有状态
connected|除listening和closed
synchronized|除syn-sent

表达式|意义
-|-
src \[IP\[/掩码位数\]\]\[:端口\]|本地位置
dst \[IP\[/掩码位数\]\]\[:端口\]|对端位置
## ssh 安全shell
```sh
ssh [选项] 目标 [命令]
```
选项|意义
-|-
-i 文件|指定使用的客户端配置
-p 端口|指定端口
-X|传送X11，需要DISPLAY=localhost:0
-x|不传送X11

客户端全局配置文件/etc/ssh/ssh_config，定义了默认的私钥（包括~/.ssh/id_rsa）  
客户端用户配置文件~/.ssh/config  
服务器配置文件/etc/ssh/sshd_config
### 客户端用户配置
```python
Host 别名
  HostName IP地址或域名 # 若无HostName，则Host应为全名
  Port 端口，默认22
  User 默认用户名
  IdentityFile ~/.ssh/私钥 # 免密登录
```
配置免密登录时将公钥上传到服务器  
登录服务器，追加公钥到~/.ssh/authorized_keys  
确保.ssh权限为700，authorized_keys权限为600
```sh
ssh [用户名@]别名
```
### 服务器配置
```python
ListenAddress IP地址 # 监听地址
AllowGroups root sudo # 允许登录的组
PermitRootLogin no # 禁止root登录
PasswordAuthentication no # 禁止密码登录
X11Forwarding yes # 传送X11信息
ClientAliveInterval 60 # 保持连接
AuthorizedKeysFile .ssh/authorized_keys # 公钥存放位置，以家目录为工作目录
```
## ssh-keygen 生成公私钥对
```sh
ssh-keygen -t rsa [-f 私钥名] [-C 注释]
```
若未指定私钥名，则交互式指定
## update-ca-certificates 更新证书列表
* 将证书置于`/usr/share/ca-certificates`
* 在`/etc/ca-certificates.conf`中以`/usr/share/ca-certificates`为工作目录添加或删除证书
```sh
update-ca-certificates
```
* 上述命令修改`/etc/ssl/certs`下的软链接
## wget 下载文件
```sh
wget [选项] [URL]
```
选项|意义
-|-
-c|继续之前的下载
-i 文件|从文件按行读取URL
-P 目录|下载文件存放目录，默认`.`

* `-c`比较URL末端和当前目录下的同名文件，视文件为下载的前一部分，并从文件大小的字节开始请求服务器
