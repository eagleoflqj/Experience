```sh
gcc [选项] [源文件1 ...]
```
# 输出类型
* 默认四个阶段：预处理、编译、汇编、链接
* 忽略无需执行操作的文件
* 头文件默认输出.h.gch
* 可执行文件默认输出a.out
* GCC不链接C++库
## -c 不执行链接
* 默认输出.o
## -S 预处理及编译
* 源文件默认输出.s
## -E 只预处理
* 输出到stdout

## -x 语言 指定源文件语言
* 语言取值
c
c-header
cpp-output
c++
c++-header
cpp-output
assembler
* 与阶段选项组合可能无意义
* 不指定则根据扩展名判断

扩展名|意义
-|-
.c|执行预处理的C源码
.i|不执行预处理的C源码
.h|将被转换为预编译头的头文件
.s|汇编文件
.S|执行预处理的汇编文件
## -o 文件 指定输出文件
## -v 在stderr输出各阶段执行的命令以及编译器版本
## -### 同-v但不执行命令，参数用引号包括
## --version GCC版本
## -pass-exit-codes 保留最大返回值
* 不指定则出错一律返回1
* C/C++前端内部错误返回4
## -pipe 在阶段间使用管道
* 不指定则使用临时文件
## @文件 读取文件中的选项
* 选项空白符分隔，插入到当前位置
* @可以递归
# 标准
## -ansi
* 等价于c90或c++98
* 预定义`__STRICT_ANSI__`
## -std=标准
C标准|`__STDC_VERSION__`
-|-
c90 gnu90|无
iso9899:199409|199409
c99 gnu99|199901
c11 gnu11|201112
c17 gnu17（默认）|201710

C++标准|`__cplusplus`
-|-
c++98 gnu++98|199711
c++11 gnu++11|201103
c++14 gnu++14（默认）|201402
c++17 gnu++17|201703
# 警告
## -w 不输出警告
## -Werror 视所有警告为错误
## -pedantic 警告ISO标准外的特性
* 不完全支持，不能作为标准检查
## -pedantic-errors 视ISO标准外的特性为错误
## -Wall 显示大多数警告
## -Wextra 显示额外警告
# 调试
## -g[等级] 加入调试信息
等级|意义
-|-
0|无调试信息，相当于不指定-g
1|函数、外部变量、行号，无局部变量
2|默认
3|额外信息，如宏定义
# 优化
## -O0或不指定 无优化
* 编译开销最小，调试可预期
## -O或-O1 一级优化
* 耗时长，较大函数占用很大内存
## -O2 二级优化
* 耗时更长，生成的代码效率更高
## -O3 三级优化
* 在O2基础上增加代码长度以提升效率
## -Os 优化代码长度
* 关闭O2中通常增加代码长度的选项
* 开启-finline-functions
## -Ofast 优化速度
* 在O3基础上放弃一些标准
* 开启-ffast-math，导致NaN等问题
## -Og 优化调试体验
* 优于O0，后者禁用了一些收集调试信息的compiler passes
# 插桩
* 收集统计信息分析代码瓶颈、代码覆盖率、基于性能分析的优化
* 运行时程序检查，如非法指针解引用、数组越界、缓冲区溢出攻击、C++ Vtable攻击
## -finstrument-functions 在所有函数开始、结束插桩
* 需要实现以下两函数，其中`this_fn`是当前函数指针
* `void __cyg_profile_func_enter (void *this_fn, void *call_site)`
* `void __cyg_profile_func_exit  (void *this_fn, void *call_site)`
## -finstrument-functions-exclude-file-list=文件1,... 不在开始、结束插桩的文件
## -fstack-protector 在脆弱函数的栈放入金丝雀
## -fstack-protector-all 在所有函数的栈放入金丝雀
# 预处理
## -D宏[=定义] 定义宏
* 定义默认1
* 可能需要根据shell的规则使用引号
## -U宏 取消宏定义
## -pthread 定义POSIX线程库需要的额外的宏
* 需要配合链接的`-pthread`
## -dM 输出所有宏定义
* 需要指定`-E`
### 查看默认宏定义
```sh
echo | gcc -E -dM -
```
## -trigraphs 支持三字符序列
# 链接
## -l库 链接指定的库
* 此选项由GCC直接传给链接器
* 链接`lib库.a`或`lib库.so`，同时存在时链接so，除非指定`-static`
* 链接器按顺序搜索定义，因此对于`foo.o -lz bar.o`，若bar.o引用了z库的符号，则无法解析
## -pie 生成动态链接PIE
* 需要编译阶段生成位置无关代码
## -pthread 链接POSIX线程库
* 需要配合预处理的`-pthread`
# 搜索目录
## -I 目录 添加头文件目录
## -iquote 目录
## -isystem 目录
## -idirafter 目录
* `-I`、`-isystem`和`-idrafter`用于<>或""引用的头文件
* `-iquote`只用于""引用的头文件
### 搜索顺序
* （仅""）文件所在目录
* （仅""）`-iquote`指定的目录按顺序
* `-I`指定的目录按顺序
* `-isystem`指定的目录按顺序
* 标准系统目录
* `-idirafter`指定的目录按顺序
### 查看当前选项下头文件搜索目录
```sh
echo | gcc [选项] -Wp,-v -xc - -fsyntax-only
```
## -L 目录 添加-l搜索的目录
* 多个`-L`按顺序搜索
# 代码生成
## -fpic 生成位置无关代码
* 可用于共享库
* 全局偏移表超过某些机器的长度限制时报错
* 预定义`__pic__`和`__PIC__`为1
## -fPIC 生成位置无关代码
* 类似-fpic，但避免长度限制
* 预定义`__pic__`和`__PIC__`为2
## -fpie/-fPIE 生成位置无关代码
* 只用于可执行文件
* 对全局偏移表长度的限制分别类似-fpic、-fPIC
* 分别预定义相同的`__pic__`、`__PIC__`、`__pie__`和`__PIE__`为1或2
# 开发者
## -dumpmachine 输出编译器目标机器
## -dumpspecs 输出编译器内置规格
## -print-file-name=库 输出链接时使用的库路径
## -print-libgcc-file-name 同`-print-file-name=libgcc.a`
## -save-temps[=目录] 按源文件名保存中间文件
* 默认写入工作目录
## -save-temps=obj 按目标文件名保存中间文件
* 需要指定`-o`
