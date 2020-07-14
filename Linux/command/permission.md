# 角色
## chage 修改密码过期信息
```sh
chage [选项] 用户名
```
选项|意义
-|-
无|交互设置
-d YYYY-MM-DD或UNIX天数|设置shadow第3个字段
-E YYYY-MM-DD或UNIX天数|设置shadow第8个字段
-I 天数|设置shadow第7个字段
-l|列出过期信息
-m 天数|设置shadow第4个字段
-M 天数|设置shadow第5个字段
-W 天数|设置shadow第6个字段
## chfn 
```sh
chfn [选项] [用户名]
```
选项|意义
-|-
无|交互设置
-f 全名|设置全名
-h 电话|设置宅电
-o 字符串|设置其他（shadow-utils）或办公室（util-linux）
-p 电话|设置工作电话（util-linux）
-r 办公室|设置办公室（shadow-utils）
-w 电话|设置工作电话（shadow-utils）
* 默认当前用户
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
## chpasswd 设置密码
```sh
chpasswd
```
* 从标准输入按行读入`用户名:密码`
## chsh 设置登录shell
```sh
chsh [选项] [用户名]
```
选项|意义
-|-
无|交互设置
-l|列出可用登录shell（util-linux）
-s 登录shell|指定登录shell
* 默认当前用户
## finger 查询用户信息
```sh
finger [选项] [用户名或姓名]
```
选项|意义
-|-
-l|详细信息，有参数时默认
-m|视参数为用户名，否则可以匹配`/etc/passwd`中的姓名
-s|一般信息，无参数时默认
* 无参数则为所有在线用户
* Plan展示`~/.plan`内容
## gpasswd 设置组管理员
```sh
gpasswd [选项] 组名
```
组管理员选项|意义
-|-
无|设置新密码
-a 用户名|加入用户进组
-d 用户名|将用户移出组
-r|删除密码（只允许组员newgrp）

root选项|意义
-|-
-A 用户名1\[,...]|设置组管理员
-M 用户名1\[,...]|设置组成员
## groupadd 添加用户组
```sh
groupadd [选项] 组名
```
选项|意义
-|-
-g GID|指定GID
-r|创建系统组
## groupdel 删除用户组
```sh
groupdel [-f] 组名
```
* `-f`：强制删除主组（`/etc/passwd`仍保留GID）
## groupmod 修改用户组
```sh
groupmod 选项 组名
```
选项|意义
-|-
-g|同`groupadd -g`
-n 新组名|修改组名
## groups 查看用户所属用户组
```sh
groups [用户名]
```
* 默认当前用户
* 输出用户名:用户组
## grpck 检查用户组信息完整性
```sh
grpck
```
## grpconv 将/etc/group同步到/etc/gshadow
```sh
grpconv
```
## id 输出用户信息
```sh
id [选项] [用户名或UID]
```
选项|意义
-|-
无|全部信息
-g|主组GID
-G|所有组GID
-u|UID
## newgrp 以另一个主组启动新shell
```sh
newgrp [-] [组名]
```
* `-`：启动登录shell
* 默认`/etc/passwd`中的主组
* 非组员必须提供组密码
## passwd 设置密码
```sh
passwd [选项] [用户名]
```
* 默认为当前用户

用户选项|意义
-|-
无|设置新密码
--stdin|（发行版特定）接收管道传来的密码

root选项|意义
-|-
-d|删除密码（隐含取消锁定状态）
-e|立即过期
-i 天数|设置shadow第7个字段
-l|锁定（禁用密码，在`/etc/shadow`密码项前添加`!`）
-n 天数|设置shadow第4个字段
-S|查看shadow信息
-u|解锁（恢复密码）
-w 天数|设置shadow第6个字段
-x 天数|设置shadow第5个字段
## pwck 检查用户信息完整性
```sh
pwck
```
## pwconv 将/etc/passwd同步到/etc/shadow
```sh
pwconv
```
## useradd 添加用户
```sh
useradd [选项] 用户名
useradd -D # 查看默认设置
```
选项|意义|默认值
-|-|-
-b `BASE_DIR`|指定家目录的父目录|配置文件中`HOME`，或`/home`
-c 字符串|指定`/etc/passwd`第5列|无
-d 家目录|指定家目录|`BASE_DIR/用户名`
-e YYYY-MM-DD|指定失效日期|配置文件中`EXPIRE`，或`/home`
-f 天数|指定密码到期后的宽限时间，0为立即禁用，-1不禁用|配置文件中`INACTIVE`，或-1
-g 组名或GID|指定主组|若`USERGROUPS_ENAB`为yes，则与用户同名；否则为配置文件中`GROUP`，或100
-G 组名1或GID1\[,...]|指定所属其他组
-k 框架目录|指定`-m`时的框架目录|配置文件中`SKEL`，或`/etc/skel`
-m|显式指定建立家目录|由`CREATE_HOME`决定
-M|显式指定不建立家目录|由`CREATE_HOME`决定
-r|创建系统账户，无过期信息，不自动建立家目录
-s 登录shell|指定登录shell|配置文件中`SHELL`，或空
-u UID|指定UID|不小于`UID_MIN`并大于所有其他用户UID的第一个
* 配置文件`/etc/default/useradd`
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
* 若被删用户是主组的唯一用户，且`USERGROUPS_ENAB`为yes，会同时删除主组
## usermod 修改用户
```sh
usermod 选项 用户名
```
选项|意义
-|-
-aG 组名1或GID1\[,...]|添加所属其他组
-c|同`useradd -c`
-d|同`useradd -d`
-e|同`chage -E`
-f|同`useradd -f`
-g|同`useradd -g`
-G|同`useradd -G`
-l 新用户名|修改用户名
-L|同`passwd -l`
-s|同`useradd -s`
-u|同`useradd -u`
-U|同`passwd -u`
## visudo 编辑sudo配置文件
```sh
visudo
```
* 退出时自动检查语法
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
* `chmod o+t`后，即使拥有目录w权限，也无法删除其他用户的文件
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
-b|删除全部细分权限
-d|设置默认细分权限，只针对目录
-k|删除默认细分权限
-m ACL项|修改细分权限
-R|递归设置
-x 不带权限的ACL项|删除细分权限
--test|输出设置后的结果但不设置
### ACL项
* \[d:]u:\[用户]:权限
* \[d:]g:\[用户组]:权限
* \[d:]o:权限
* \[d:]m:权限（设置有效权限，即对其他ACL权限的限制，可能被其他设置更改）
* 不指定用户（组）则默认所属用户（组）
* 默认细分权限：目录下新建文件、目录继承该细分权限
### 权限
* rwx或0-7
