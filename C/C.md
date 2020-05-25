# Pointers on C
## 2
### 三字符序列
三字符|字符
-|-
??(|[
??)|]
??!|&#124;
??<|{
??>|}
??'|^
??=|#
??/|\
??-|~
### 转义
* `\ooo`，ooo为1至3位八进制数，不超过\377（255）
* `\xhh`，hh为1至2位十六进制数
## 3
* `limits.h`规定了整型范围
* 可移植：char存储字符应位于singed char和unsigned char交集
* char参与运算应显式声明为signed char或unsigned char
* `-整数`不是整型字面量，而是常量表达式；因此`INT_MIN`定义为`-INT_MAX-1`
* `L''`宽字符常量
* `float.h`规定了浮点数特性
### 全局变量
* 声明：`int a;`
* 定义：`int a = 1;`
* 所有文件中均无初值则初始化为0
* 建议声明处添加`extern`
### 静态变量
* `static`作用于全局：仅当前文件访问
* `static`作用于函数内：仅当前函数访问，只初始化一次
## 5
* 可移植：标准未定义有符号数的右移是算术/逻辑
* 标准未定义负数或超宽位移量的行为
* 连续赋值时各变量类型不同，发生截断
* `EOF`超过signed或unsigned char的表示范围，应用int存储
* 隐式类型转换：算术、位运算将字符型、短整型提升至整型
* 算术转换：按int、unsigned、long、unsigned long、float、double、long double排序转换
* int运算可能溢出，不可赋给long
* int转float可能丢失精度，超过int范围的float转int结果未定义
* 可移植：表达式求值顺序不完全由运算符优先级决定，因此要避免副作用
### 逗号表达式
```c
statement;
while(condition) {
    do_something;
    statement;
}
```
可简写为
```c
while(statement, condition) {
    do_something;
}
```
## 6
* 解引用野指针可能会导致段错误（非法地址）、总线错误（在强制要求对齐的机器上访问未对齐地址），或对合法地址的错误修改
* `*NULL`结果未定义
* 指针大小比较、运算只在同一数组及其后一个元素合法
### 倒序循环
```c
// 正确
for(p = a + n; p > a;) {
    *--p = 0;
}
// 错误
for(p = a + (n - 1); p >= a; --p) {
    *p = 0;
}
```
## 7
* 编译器根据函数原型自动对实参、返回值做类型转换
* 对没有原型的函数，编译器对实参执行缺省参数提升（字符型、短整型实参提升至int，float实参提升至double），且在返回int的假定下转换返回值
* 为保证声明与定义一致，函数原型应仅出现一次（头文件或定义所在文件的全局作用域）
* 无参函数应显式声明参数void
* 形参、实参数不同时，标准未定义匹配方式
* 对可变参数函数，编译器对实参执行缺省参数提升，`va_arg`第二个参数需为提升后的类型
## 8
* 数组不完整初始化，最后几个元素初值为0
* 定义数组时只有最高维长度可以根据初始化列表自动推断
* 数组常量地址与值相同（类型不同），读取元素访问一次内存；指针变量地址与值不同，读取元素访问两次内存
```c
// a.c
int x[1];
int *y = x;
// b.c
extern int *x;
extern int y[];
// 不变量：&x、&y
```
b.c中表达式|编译器根据声明的翻译|反应到a.c的定义
-|-|-
x\[0]|\*x = \*(\*(&x))|\*(*x) = *0
y\[0]|\*y = \*(&y)|y = x
## 9
* 标准保留了所有str开头的函数名
* `size_t strlen`返回字符串长度
* `strcpy`、`strcat`内存重叠时结果未定义
* `int strcmp`结果不相等时，标准只规定了返回值的符号
* `strncpy(dst, src, n)`恰好复制`n`字节，若`strlen(src) < n`则补`'\0'`，若`strlen(src) >= n`则`dst`可能不以`'\0'`结尾
* `strncat(dst, src, n)`复制`min(strlen(src), n)`个字符+一个`'\0'`
* `char *strchr(s, int c)` / `strrchr(s, int c)`返回字符第一次/最后一次出现的地址或NULL，`c`转换为char
* `char *strpbrk(s, charset)`返回`charset`中字符第一次出现的地址或NULL
* `char *strstr(s, sub)`返回子串第一次出现的起始地址或NULL
* `size_t strspn(s, charset)`返回`s`的开头匹配`charset`中字符的长度或`strlen(s)`
* `size_t strcspn(s, charset)`返回`s`的开头不能匹配`charset`中字符的长度（即`strpbrk(s, charset) - s`）或`strlen(s)`
* `char *strerror(errno)`返回`errno`的错误描述，不可修改
* 可移植：`ctype.h`中的函数适用任何字符集（如EBCDIC）
* `memcpy`效率更高但内存重叠时结果未定义，`memmove`可处理内存重叠
* `memcmp(s, t, n)`逐字节比较`unsigned char`值
* `void *memchr(s, int c, n)`返回`(unsigned char)c`第一次出现的地址或NULL
* `memset(s, int c, n)`把`s`开始的`n`字节设为`(unsigned char)c`
### strtok
* `char *strtok(char *s, const char *charset)`
* 若`s`非空，将`s`用`charset`中的字符分割，返回指向第一个非空子串的指针，并将结尾设为`\0`（`s`必须为可变字符串），或返回NULL
* 若`s`为空，继续上一次调用，返回下一个非空子串或NULL
* 非VC版本的实现线程不安全
```c
char line[] = " 0, 1, 2,";
char *charset = " ,";
for(char *token = strtok(line, charset);
  token; token = strtok(NULL, charset)) {
    puts(token);
}
```
## 10
* `stddef.h`中`offsetof(type, member)`返回成员在结构中的偏移量
* 可移植：避免使用位字段（int位字段的符号，字段中位的最大长度，成员存储顺序，跨越int边界）
* 联合的初始化只能为第一个成员
## 11
* `calloc`用0初始化空间，`malloc`不保证初始化
* `void *realloc(p, n)`若`p`为空，同`malloc`；若`p`非空，试图在原位置调整分配空间，如果无法满足则开辟新空间，复制原空间的内容，释放原空间
## 12
* 链表不使用头节点，利用指向指针（而非节点）的指针消除头部判断
## 14
预定义符号|样例|意义
-|-|-
\_\_DATE__|"May 24 2020"|编译日期
\_\_FILE__|"name.c"|（传递给编译器的）文件名
\_\_LINE__|25|当前行号
\_\_STDC__|1|编译器是否遵循ANSI C（对其后的标准不确定）
\_\_TIME__|"22:12:00"|编译时间
* `#宏参数`表示代入该参数的字符串
* 宏定义中`##`可用来连接宏参数或符号，生成token
* 使用`#if defined(宏)`处理逻辑关系
* 嵌套条件编译，`#endif`后加注释表明关闭的`#if`
* 标准规定至少支持8层头文件嵌套
* `#error 信息`报错
* `#line 行号 ["文件"]`重设下一行的`__LINE__`和（可选）`__FILE__`
* 可移植：`#progma`被不识别的预处理器忽略，其解释也是预处理器相关的
* 单独的`#`被预处理器忽略
