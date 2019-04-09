# 安装
官网提示安装
# 启动
```sh
mysqld --user=root
```
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