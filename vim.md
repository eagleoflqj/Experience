# 基础用法
## 打开或创建文件
```sh
vim 文件
```
## 命令
|||
-|-
i|切换到插入状态
Esc|切换到命令状态
o|添加新行并切换插入状态
x|删除当前字符
yy|复制当前行
p|在当前行下粘贴行
P|在当前行上粘贴行
dd|删除当前行
u|撤销
:q|退出
:!q|不保存退出
:wq|保存并退出
:set nu|显示行号
:行号|跳转到指定行
/单词|查找
n|查找下一个
N|查找上一个
h|光标左移
j|光标下移
k|光标上移
l|光标右移
# 问题
## 中文乱码
/etc/vimrc中添加
```sh
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
```