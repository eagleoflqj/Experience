# Pointers on C
## 2
* 标准要求区分（但不必区分大小写）内部标识符的前31字符，和外部（链接）标识符的前6个字符（C17分别为63和31）
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
* `\xhh`，hh为多位十六进制数（可用于宽字符），若值超过表示字符的范围，结果未定义
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
while (condition) {
    do_something;
    statement;
}
```
可简写为
```c
while (statement, condition) {
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
for (p = a + n; p > a;) {
    *--p = 0;
}
// 错误
for (p = a + (n - 1); p >= a; --p) {
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
* 连续调用之间不能插入其他`strtok`调用
* 非VC版本的实现线程不安全
```c
char line[] = " 0, 1, 2,";
char *charset = " ,";
for (char *token = strtok(line, charset);
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
## 15
* 当一个库函数失败时，`errno.h`中的`errno`被更新，可用`stdio.h`中的`perror(s)`输出`s: 错误描述`；`errno`是线程私有的；当库函数返回值可以指示错误时，不用提前置`errno`为0
* `stdlib.h`中的`exit(int status)`终止程序并返回`status`，`status`可为`EXIT_SUCCESS`或`EXIT_FAILURE`等
* 标准规定文本流的文本行至少允许254个字符
* 操作系统文本行结束方式不同，库函数负责提供统一的内部形式
* `FOPEN_MAX`为最多可以打开的文件流数（含3个标准流），不少于8
* `FILENAME_MAX`说明一个字符数组至少多大，以容纳编译器支持的最长文件名
* `FILE *fopen(name, mode)`打开流，`mode`带`"b"`表示二进制（在POSIX上无效果，只用于兼容ANSI C）

mode|意义
-|-
r|从头读
r+|从头读写
w|从头写，截断原文件或新建文件
w+|同w，且可读
a|追加原文件或新建文件
a+|同a，且可读，但读的位置不确定
* `int fclose(fp)`关闭流，成功返回0，应检查返回值
* `FILE *freopen(name, mode, fp)`关闭流并重新打开，失败返回NULL
* `fgetc(fp)`、`fputc(c, fp)`是函数，`getc(fp)`、`getchar(void)`、`putc(c, fp)`、`putchar(c)`可以是宏
* `int ungetc(c, fp)`将`c`放回流中，返回`c`或`EOF`；每个流保证能放回1个字符
* `char *fgets(buf, n, fp)`保留换行符，空字符结尾，返回`buf`或NULL
* `int fputs(buf, fp)`不添加换行符，返回非负数或`EOF`
* `char *gets(buf)`不保留换行符；无法被安全使用，已于C11移除
* `int puts(buf)`添加换行符
* `scanf`格式码：`%`+`*`（可选，丢弃匹配值）+宽度（可选）+限定符+类型

类型\限定符|h|l|L
-|-|-|-
d i n|short|long
o u x|unsigned short|unsigned long
e f g||double|long double
* `printf`中`*`可用于指定最小宽度、精度
* `size_t fread(buf, size, count, fp)`、`size_t fwrite(buf, size, count, fp)`返回实际读取、写入的`count`
* `fflush(fp)`强制将输出缓冲区的内容物理写入，返回0或`EOF`
* `long ftell(fp)`返回流的当前位置
* `int fseek(fp, long offset, int from)`设置流的位置，会丢弃`ungetc`放回的字符

from|意义
-|-
SEEK_SET|起始位置起，offset非负，对文本文件offset应为`ftell`返回值以避免行末映射问题
SEEK_CUR|当前位置起，不宜用于文本文件
SEEK_END|结尾位置起，对文本文件offset应为0，对二进制文件不一定支持
* `void rewind(fp)`设置为起始位置，清除流的错误标志
* `int fgetpos(fp, fpos_t *pos)`和`int fsetpos(fp, fpos_t const *pos)`获取、设置流位置，但`fpos_t`不一定为整型，不能用于计算
* `void setvbuf(fp, buf, mode, size)`修改缓冲区及缓冲方式，需要在打开流后其他操作前调用；大的`size`最好为`BUFSIZ`整数倍
* 可移植：如果`buf`为NULL，`size`应为0

mode|意义
-|-
_IOFBF|全缓冲，达到size后输出
_IOLBF|行缓冲，达到size或行尾后输出
_IONBF|无缓冲，忽略buf和size参数
* `void setbuf(fp, buf)`等价于`setvbuf(fp, buf, buf ? _IOFBF : _IONBF, BUFSIZ);`，`buf`长度不小于`BUFSIZ`
* `int feof(fp)`到达结尾时返回真，否则返回0
* `int ferror(fp)`流出错时返回真，否则返回0
* `void clearerr(fp)`清除流的错误标志
* `FILE *tmpfile(void)`创建`wb+`临时文件，关闭或程序退出时自动删除
* `char *tmpnam(buf)`生成临时文件名，若`buf`为NULL则返回一个指向静态字符串的指针，否则`buf`长度应不小于`L_tmpnam`；调用不超过`TMP_MAX`次保证不重名；多任务系统不保证无冲突
* `int remove(name)`删除文件，成功返回0；若文件被打开，结果取决于编译器
* `int rename(name, newname)`重命名文件，成功返回0；若新文件已存在，结果取决于编译器
## 16
### stdlib.h
* `int abs(int)` / `long labs(long)`对`INT_MIN` / `LONG_MIN`结果未定义
* `div_t div(int n, int d)` / `ldiv_t ldiv(long n, long d)`返回结构，包含int / long的`quot`和`rem`，字段顺序不确定；商向0取整（而`/`运算符对负数的处理取决于编译器）
* `int rand(void)`产生0到`RAND_MAX`（至少为32767）的伪随机数
* `void srand(unsigned)`设置随机数种子
* `int atoi(s)`、`long atol(s)`转换整数，不要求设置`errno`，返回值无法说明错误，不建议使用
* `long strtol(s, char **next, base)`、`unsigned long strtoul(s, char **next, base)`转换整数；子串不含合法数值返回0，溢出返回`LONG_MAX` / `LONG_MIN`或`ULONG_MAX`并设置`ERANGE`；调用前应设置`errno = 0`；若`next`非空，`*next`将指向已转换子串后的字符；`base`为0则`s`应为整型字面量，否则`base`应为2至36
* `double strtod(s, char **next)`转换合法浮点字面量，类似`strtol`（上溢返回`HUGE_VAL`）
* `double atof(s)`同`strtod(s, NULL)`，但不要求设置`errno`
* `void abort(void)`发送`SIGABRT`信号，不正常终止程序
* `void atexit(void (*f)(void))`注册正常退出时调用的函数，后注册的先调用；若`atexit`注册的函数调用了`exit`，结果未定义
* `char *getenv(name)`获取环境变量，返回不可更改的字符串或NULL
* `int system(command)`在shell执行命令，返回值因实现而异；若`command`为空，则存在可用shell时返回非零值
* `void qsort(void *p, size_t n, size_t el_size, int (*compare)(void const *, void const *))`生序排序`n`元数组`p`，每个元素占`el_size`字节
* `void *bsearch(void const *key, void const *p, size_t n, size_t el_size, int (*compare)(void const *, void const *))`对已排序的数组二分搜索同类型的元素`key`，找到则返回其指针，否则返回NULL；对未排序数组结果未定义
### math.h
* 数学函数可以但不要求设置`errno`
* 定义域错误设置`EDOM`
* 值域上溢返回`HUGE_VAL`，下溢返回0；是否设置`ERANGE`取决于编译器
* `double frexp(double v, double *e)`令`v = frac * 2^exp`（`0.5 <= frac < 1`），设置`*e = exp`并返回`frac`
* `double ldexp(double frac, double exp)`返回`frac * 2^exp`
* `double modf(double v, double *i)`令`v = ipart + fpart`（`ipart`、`fpart`和`v`同号），设置`*i = ipart`并返回`fpart`
* `double floor(double)`、`double ceil(double)`
### time.h
* `clock_t clock(void)`返回从程序开始执行，CPU消耗的时间，若无法提供或超长则返回-1；为得到秒数需要除以`CLOCKS_PER_SEC`
* `time_t time(time_t *r)`返回当前日期和时间，若`r`非空则同时存储到`*r`
* 可移植：标准未规定`time_t`的单位和意义，常实现为1970年1月1日00:00:00以来的秒数，因此有32位2038问题
* `char *ctime(time_t const *t)`返回指向静态时间字符串（如`"Tue May 26 22:01:15 2020\n"`）的指针，可能用`asctime(localtime(t))`实现
* `double difftime(time_t t1, time_t t2)`返回`t1 - t2`的秒数
* `struct tm *gmtime(time_t *t)`、`struct tm *localtime(time_t *t)`返回指向静态时间结构的指针，结构为`t`的UTC表示、本地表示，字段均为int

struct tm字段|范围
-|-
tm_sec|0-61（C99修正为0-60）
tm_min|0-59
tm_hour|0-23
tm_mday|1-31
tm_mon|0-11
tm_year|0-?（0为1900年）
tm_wday|0-6（0为星期日）
tm_yday|0-365
tm_isdat|-1未知，0非夏令时，1夏令时
* `char *asctime(struct tm const *t)`类似`ctime`但参数不同
* `size_t strftime(char *s, size_t size, char *format, struct tm const *t)`转换时间为字符串，若（数组足够大的话）`strlen(s) + 1`不超过`size`则正确设置`s`并返回`strlen(s)`，否则`s`未定义并返回0
* 可移植：`strftime`不使用编译器自定义的格式
* `time_t mktime(struct tm *t)`返回`struct tm`对应的`time_t`，转换时忽略`tm_wady`和`tm_yday`，并对`t`规范化
* 操作静态内存的函数调用会互相影响
### setjmp.h
* `int setjmp(jmp_buf env)`保存当前状态（堆栈指针、程序计数器）到`state`，返回0
* `void longjmp(jmp_buf env, int val)`相当于`setjmp`返回`val`（若`val`为0，则相当于返回1）
* 若调用`setjmp`的函数已返回，不可调用`longjmp`
* 调用`longjmp`前应释放资源
* 非`volatile`局部变量如果在调用`setjmp`后修改，通过`longjmp`返回后该变量不一定取新值
```c
#include <stdio.h>
#include <setjmp.h>
jmp_buf env;
void second(void) {
    puts("second");
    longjmp(env, 1);
}
void first(void) {
    second();
    puts("first");
}
int main() {
    volatile int vlocal = 0;
    int local = 0;
    if (!setjmp(env)) {
        vlocal = local = 1;
        first();
    } else {
        printf("%d %d\n", vlocal, local);
    }
    puts("main");
    return 0;
}
/* 输出
second
1 1（无优化）或1 0（-O）
main
*/
```
### signal.h
信号|意义
-|-
SIGABRT|程序请求异常终止
SIGFPE|算术错误，如整数除以0
SIGILL|不识别的指令
SIGSEGV|非法访问内存
SIGINT|交互式中断程序
SIGTERM|异步终止程序
* 可移植：标准不要求实现上述信号
* `int raise(int sig)`向当前线程发送信号，成功返回0，否则-1
* `void (*signal(int sig, void (*handler)(int)))(int)`对信号`sig`设置新`handler`并返回旧处理函数，失败返回`SIG_ERR`；`handler`也可以为`SIG_DFL`（默认）或`SIG_IGN`
* 对于一个设置了处理函数的信号，当信号到来时，系统可能会先恢复默认的处理函数（对后续信号起效），再调用设置的处理函数，因此在处理函数中可能要重新调用`signal`
* 对异步信号（非`abort`或`raise`引起），处理函数不应调用除`signal`外的库函数，除向一个`volatile sig_atomic_t`静态变量赋值外不应访问其他静态变量；程序其余部分可检查这个变量
* 标准说明处理函数可以调用`exit`、非`SIGABRT`的处理函数可以调用`abort`，但这两个库函数在异步信号中仍可能无法正常运行，这种情况下程序将终止但可能破坏数据
* `sig_atomic_t`是CPU可以原子访问的类型
* `volatile`使编译器不根据“相邻语句中的同一变量值相同”优化
* 处理函数返回时，程序从信号发生时的位置继续，但`SIGFPE`除外，其返回效果未定义
### stdarg.h
* `int vprintf(format, va_list ap)`、`int vfprintf(fp, format, va_list ap)`、`int vsprintf(buf, format, va_list ap)`调用前需要用`va_start`初始化`ap`，调用后需要调用`va_end`
### assert.h
* `void assert(int expression)`宏当`expression`为假时输出源文件名和行号并退出
* 定义`NDEBUG`可丢弃所有`assert`
### locale.h
* `char *setlocale(int category, const char *locale)`设置本地化信息，`locale`为空时返回当前locale，否则尝试设置新locale，成功返回`locale`，失败返回NULL（原locale不改变）

category|意义
-|-
LC_ALL|所有locale
LC_COLLATE|对照序列，决定字符如何排序
LC_CTYPE|字符分类
LC_MONETARY|货币
LC_NUMERIC|非货币数字
LC_TIME|strftime
* `struct lconv *localeconv(void)`返回当前locale下数字和货币格式信息
### string.h
* `int strcoll(const char *s1, const char *s2)`同`strcmp`但根据当前`LC_COLLATE`比较
* `size_t strxfrm(char *dst, const char *src, size_t n)`将`src`按当前locale转换并存储（最多`n`个字符，含`'\0'`）到`dst`，使对`src`进行`strcoll`等价于对`dst`进行`strcmp`
## 17
* 使用宏实现C++模板
* 使用`void *`实现Java泛型
## 18
* 外部标识符
* 某些编译器表达式语句修改寄存器值，当寄存器被用于返回值且`return`语句无返回值时，可能会返回错误值
* 通过测试各函数调用次数及时间优化程序
