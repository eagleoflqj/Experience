# 概念
## pattern space
每读取一行，删去结尾换行符，放入pattern space处理，输出并换行，清空pattern space
## hold space
通过命令存取hold space中的文本
# 用法
```sh
sed [选项] '当无其他命令时将此参数解释为命令' [文件1 ...]
```
选项|意义
-|-
-e 命令|指定命令
-f 文件|从文件按行读取命令
-i|直接修改文件
-n|不自动输出pattern space
-r|使用扩展正则表达式语法
* 文件不存在则为标准输入

命令|意义
-|-
\[`m`\[,`n`]]\[!\]命令|命令对`m`到`n`行生效（1开始，$最后），!表示不生效行数
=|输出当前行号并换行
a 字符串|在当前行后插入一行
c 字符串|替换指定行
d|删除pattern space，开始处理下一行
g或G|复制/追加hold space到pattern space
h或H|复制/追加pattern space到hold space
i 字符串|在当前行前插入一行
p|输出指定行，通常配合`-n`
s/正则表达式/替换/\[g\]|正则替换，g表示每行全部替换
x|交换pattern space和hold space
* `a`、`c`、`i`若要插入多行则每行应以`\`结尾再换行
```sh
sed '1a 行1\
行2' 文件
```
