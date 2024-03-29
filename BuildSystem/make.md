# 命令
```sh
make [选项] [目标] [变量1=值1 ...]
```
选项|意义
-|-
-e|环境变量覆盖make变量
-f 文件|指定make文件，默认Makefile
-j \[并行度]|不指定`-j`则串行，不指定并行度则无上限
-r|禁用内置规则
DESTDIR=绝对路径|make install的根目录
* 目标默认Makefile中第一个目标
* 默认命令行变量>make变量>环境变量
# Makefile
## 基本格式
```Makefile
目标: [前提1 ...]
    [命令1
    ...]
```
* 若与目标同名的文件已存在且比前提文件新，则不执行命令
* 前提和命令至少指定一个
* 命令前必须为tab
* 每行命令在单独的shell执行
## 注释
每行`#`及其后内容
## 回显
* 默认执行每条命令前打印命令内容（包括注释）
* tab后加`@`关闭该行回显
* 一般用于纯注释或echo
## 通配符
与bash一致
## 模式匹配
```Makefile
%.o: %.c
```
* 目标和前提文件名的相同部分可用%匹配
* 若不指定命令，则用于禁用内置规则
## 旧式后缀规则
* 单后缀规则：`.c:`等价于`%: %.c`
* 双后缀规则：`.c.o:`等价于`%.o: %.c`
* 若指定前提，则后缀规则不适用
* 针对常见后缀有默认的旧式规则
## 变量
```Makefile
变量 = 值 # 运行时求值
变量 := 值 # 定义时求值
变量 ?= 值 # 运行时若变量为空则求值
变量 += 值 # 追加，求值时间取决于变量声明，若无声明则相当于=
```
* 调用使用`$(变量)`，Shell变量要`$$变量`
* 命令行指定的变量覆盖文件中的同名变量，变量前加`override`实现保留（`=`、`:=`、`?=`）和追加（`+=`）
* 值若有引号，则引号是值的一部分
## 自动变量
变量|意义
-|-
$@|当前目标
$<|第一个前提
$?|比目标新的前提，空格分隔
$^|所有前提，空格分隔
$*|`%`匹配的部分
$(@D)|`$@`的路径部分
$(@F)|`$@`的文件名
$(<D) $(<F)|`$<`的路径部分、文件名
## 包含
```Makefile
include 其他Makefile
```
# 内置目标
## .NOTPARALLEL 串行执行，忽略-j
## .ONESHELL 每个目标的多行命令在同一shell执行
```Makefile
.ONESHELL:
```
## .PHONY 伪目标
```Makefile
.PHONY: 目标1 [... 目标n]
```
* 声明为伪目标后，无论是否存在同名文件，均执行命令
## .SUFFIXES 添加适用旧式后缀规则的后缀
```Makefile
.SUFFIXES: [.后缀1 ...]
```
* 双后缀规则`.a.b:`应分别添加`.a`和`.b`
* 无后缀则禁用所有内置旧式后缀规则
## 条件/循环
与bash一致
## 函数
```Makefile
$(函数 参数)
```
返回标准输出
# 内置变量
程序|说明|默认值
-|-|-
AS|汇编器|as
CC|C编译器|cc
CXX|C++编译器|g++
MAKE|当前使用的make|make
RM|删除|rm -f

参数|程序
-|-
ASFLAGS|AS
CFLAGS|CC
CXXFLAGS|CXX
LDFLAGS|ld
LDLIBS|ld
# 特殊变量
## MAKEFILE_LIST 已读取的Makefile列表
* 无include时，`$(lastword $(MAKEFILE_LIST))`为当前Makefile
## .RECIPEPREFIX 替换命令前导tab
```Makefile
.RECIPEPREFIX = 字符
```
* 从设置起生效，直到下一次修改
# 内置函数
## abspath 绝对路径
```Makefile
$(abspath 路径1 [...])
```
## dir 路径去尾
```Makefile
$(dir 路径1 [...])
```
## lastword 最后一个单词
```Makefile
$(lastword 文本)
```
## patsubst 模式替换
```Makefile
$(patsubst 模式, 替换, 文本)
$(变量:模式=替换)
```
## shell 执行shell命令
```Makefile
$(shell [参数1 ...])
```
## subst 文本替换
```Makefile
$(subst 旧, 新, 文本)
```
## wildcard 将通配符替换为完整路径
```Makefile
$(wildcard 带通配符路径)
```
## word 第N个单词
```Makefile
$(word N, 文本)
```
* 从1开始，超过单词数则为空
## words 单词数
```Makefile
$(words 文本)
```
