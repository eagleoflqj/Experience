# 安装
```sh
apt install sqlite3
```
# 命令
```sh
sqlite3 [参数] [文件]
```
参数|意义
-|-
-readonly|只读方式打开
-version|查看版本
# 点命令
## .cd 修改工作目录
```
.cd 目录
```
## .open 打开文件
```
.open 文件
```
## .databases 查看当前数据库
## .tables 查看表
```
.tables [表]
```
* 表可用通配符
## .dump 输出备份语句
```
.dump [表]
```
* 表可用通配符
## .save 另存为文件
```
.save 文件
```
## .exit 退出
