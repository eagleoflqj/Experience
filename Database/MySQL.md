# 安装
## Linux
官网提示安装
## docker
```sh
docker pull mysql
```
# 启动
## Linux
```sh
mysqld --user=root
```
## docker
```sh
docker run -d \
--name mysql \
-e 环境变量=值 \
-v 存储目录:/var/lib/mysql \
mysql \
--character-set-server=utf8mb4 \
--collation-server=utf8mb4_unicode_ci
```
环境变量|意义
-|-
MYSQL_ROOT_PASSWORD|数据库root密码
# 登录
```sh
mysql [–h IP地址] -u 用户名 -p
```
## 查看初始root密码
```sh
cat /var/log/mysqld.log | grep temp
```
## 错误2002(HY000)
Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)  
/etc/mysql/my.cnf
```
[mysql]
protocol=tcp
```
# 修改密码
```sql
alter user '用户名'@'地址' identified by '密码'
```
首次登录需要修改'root'@'localhost'的密码