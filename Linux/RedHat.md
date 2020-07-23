# RPM
* 数据存储在`/var/lib/rpm`下
## 安装/更新
```sh
rpm -i|F|U [选项] 安装包
```
选项|意义
-|-
-F|仅在已安装时更新
-h|显示进度条
-i|安装
-U|安装或更新
-v|显示详情
--force|--oldpackage --replacefiles --replacepkgs
--oldpackage|允许降级
--replacefiles|覆盖冲突文件
--replacepkgs|覆盖已安装的包
--test|仅测试是否可以安装
* `-i`后接SRPM时将其解包
## 查询
```sh
rpm -qa
rpm -q [选项] 包
rpm -qp [选项] 安装包
rpm -qf 文件
```
选项|意义
-|-
-a|列出全部已安装包
-c|列出配置文件
-d|列出文档
-f|查看文件所属包
-i|列出详情
-l|列出属于包的全部目录和文件
-L|列出许可证
-R|列出依赖
--scripts|输出相关脚本
## 检验
```sh
rpm -Va
rpm -V 包
rpm -Vf 文件
```
选项|意义
-|-
-a|列出所有可能更改的文件
-f|检查文件是否更改

第一列|意义
-|-
S|大小
M|类型和权限
5|散列
D|主次设备号
L|链接
U|所属用户
G|所属组
T|修改日期
P|功能

第二列|意义
-|-
c|配置文件
d|文档
g|不包含在安装包中
l|许可证
r|README
## 卸载
```sh
rpm -e [--test] 包
```
## 重建数据库
```sh
rpm --rebuilddb
```
# Yum
## 配置
/etc/yum.repos.d/*.repo
```conf
[名称]
name=名称
baseurl=URL
enabled=0或1
gpgcheck=0或1
```
## 列出镜像仓库
```sh
yum repolist [--all|--disabled|--enabled]
```
* 默认--enabled
## 更新列表
```sh
yum update
```
## 清空缓存
```sh
yum clean all|metadata|packages
```
## 列出包
```sh
yum list [选项]
```
选项|意义
-|-
无或--all|全部
--autoremove|可自动卸载
--installed|已安装
--upgrades|可更新
## 搜索
```sh
yum search 包
```
## 详情
```sh
yum info 包
```
## 文件或命令所属包
```sh
yum provides 文件或命令
```
## 安装/更新
```sh
yum install|upgrade [-y] 包
```
## 卸载
```sh
yum remove [-y] 包
```
## 自动卸载
```sh
yum autoremove
```
## 包组
```sh
yum group list|info|install|remove
```
