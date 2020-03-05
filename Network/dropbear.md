# 源码
https://github.com/mkj/dropbear
# 编译
```sh
autoreconf
make
```
# 启动
## 首次
```sh
mkdir /etc/dropbear
./dropbear -R
```
* 连接时自动生成host key
## 后续
```sh
./dropbear
```
