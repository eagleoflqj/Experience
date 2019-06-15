# 概念
## pattern space
每读取一行，删去结尾换行符，放入pattern space处理，输出并换行，清空pattern space
## hold space
通过命令存取hold space中的文本
# 用法
```sh
sed [参数] '命令' 文件
```
参数|意义
-|-
-i|直接修改文件

命令|意义
-|-
&lt;NUMBER&gt;\[!\]&lt;COMMAND&gt;|命令生效行数（1开始，$最后），!表示不生效行数
s/&lt;REGEXP&gt;/&lt;REPLACEMENT&gt;/\[g\]|正则替换，g表示每行全部替换
=|输出当前行号并换行
d|删除pattern space，开始处理下一行
h H|复制/追加pattern space到hold space
g G|复制/追加hold space到pattern space
x|交换pattern space和hold space
