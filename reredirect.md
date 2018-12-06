程序未使用nohup且有输出，disown后关闭终端仍会结束，需要将stdout和stderr动态重定向
## 安装
```sh
git clone https://github.com/jerome-pouiller/reredirect
cd reredirect
make
make install
```
## 同时定向stdout和stderr
```sh
reredirect -m 文件 进程号
```
## 分别定向stdout和stderr
```sh
reredirect –o 输出文件 –e 错误文件 进程号
```
## 取消定向
```sh
reredirect -N -O 5 -E 3 进程号
```