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
# 内置目标
## .PHONY 伪目标
```Makefile
.PHONY: 目标1 [... 目标n]
```
* 声明为伪目标后，无论是否存在同名文件，均执行命令
## .ONESHELL 多行在同一shell执行
```Makefile
.ONESHELL:
目标: ...
    命令1
    ...
```
