# 命令
```sh
make [-f make文件] [目标]
```
* make文件默认Makefile
* 目标默认Makefile中第一个目标
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
目标和前提文件名的相同部分可用%匹配
```Makefile
%.o: %.c
```
# 内置目标
## .ONESHELL 多行在同一shell执行
```Makefile
.ONESHELL:
目标: ...
    命令1
    ...
```
## .PHONY 伪目标
```Makefile
.PHONY: 目标1 [... 目标n]
```
* 声明为伪目标后，无论是否存在同名文件，均执行命令
# 特殊变量
## .RECIPEPREFIX 替换命令前导tab
```Makefile
.RECIPEPREFIX = 字符
```
* 从设置起生效，直到下一次修改
