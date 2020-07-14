# 内核模块
## depmod 生成modules.dep和map文件
```sh
depmod [选项]
```
选项|意义
-|-
-A|仅当模块比modules.dep新时更新
-eF System.map|对比，找出无法解析的symbol
-n|不更新文件，输出到终端
## insmod 装载模块
```sh
insmod 模块文件
```
## lsmod 列出装载的模块
```sh
lsmod
```
格式化输出/proc/modules
## modinfo 模块信息
```sh
modinfo 模块或模块文件
```
## modprobe 装卸模块
```sh
modprobe [选项] 模块
modprobe -c
```
选项|意义
-|-
-c|显示配置
-r|卸载模块，同时卸载不再被依赖的模块
* 根据module.deps.bin处理依赖关系
## rmmod 卸载模块
```sh
rmmod 模块
```
