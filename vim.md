# 基础用法
## 打开或创建文件
```sh
vim 文件名
```
## 命令
|||
-----|-
i|切换到插入状态
Esc|切换到命令状态
dd|删除当前行
:q|退出
:!q|不保存退出
:wq|保存并退出
/单词|查找
n|查找下一个
N|查找上一个
# 问题
## 中文乱码
/etc/vimrc中添加
```sh
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
```