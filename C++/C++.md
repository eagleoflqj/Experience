# C++ Primer
## 1
* 读取`cin`会刷新`cout`
* 默认`cerr`和`clog`定向至标准错误；`cerr`不缓冲，`clog`缓冲
* `endl`换行并刷新缓冲区，保证程序崩溃时不丢失输出
* 标准库定义的名称全部位于`std`命名空间
* `if(cin >> value)`检测流状态，当遇到EOF或非法输入时条件为假
* 无初值实例初始化由类决定，无初值局部自动内置类型变量初值未定义
## 2
### 内置类型
* `wchar_t`至少2字节，保证能覆盖机器的最大字符集（Unicode 4字节），literal前缀`L`
* `char16_t`、`char32_t`存储Unicode字节，literal前缀为`u`、`U`
* `long double`一般用于专用浮点硬件
* 对`bool`赋非零值结果为`true`
* 对无符号类型超范围赋值导致取模，对有符号类型超范围赋值结果未定义
* decimal literal类型为`int`、`long`、`long long`中最小能容纳其值的，octal、hex literal类型为`int`、`unsigned`、`long`、`unsigned long`、`long long`、`unsigned long long`中最小能容纳其值的
* string literal的类型为`const char []`

😀（U+1F600）的前缀|可能的字符sizeof|可能的字符串sizeof
-|-|-
无|溢出|5
u8|非法|5
L|4|8
u|溢出|6
U|4|8
### 列表初始化
```cpp
int a = 0;
int a(0);
int a = {0};
int a{0};
```
* 后两种方式，若初值转换为类型可能丢失精度，编译器将报错
### 全局变量
* 全局`int a;`为定义，声明必须加`extern`
* 为避免与标准库的名称冲突，标识符不应包含两个连续的`_`，不应以`_`+大写字母开头，全局变量不应以`_`开头
* `::变量`取同名全局变量
* 全局`const`默认文件作用域，`extern const`定义全局作用域
* 可以调用函数初始化全局变量，但若函数有状态，不同编译单元内的全局变量值不确定（初始化顺序不确定）
### 引用和指针
* `int &ref = var;`引用不可更改绑定，必须被初始化
* 引用不是对象，只是另一个变量的别名，所以不能定义引用的引用、引用的指针
* 指针值可为变量地址、变量后的位置、`nullptr`、无效值，后三者时解引用结果未定义
### const
* 非`const &`只能绑定匹配类型的变量
* `const &`可以绑定到任何能转换成对应类型的表达式，此时绑定的是一个临时对象
* `const &`可以绑定到对应类型的普通变量，此时对变量的修改可以反应到引用上
* 顶层`const`：不可修改内存值；底层`const`：不可修改指向的内存值
* `l = r`对象复制时`r`的顶层`const`属性被忽略，底层`const`属性不可忽略
### constexpr
* 常量表达式：`const`且编译期值确定
* `constexpr int a = 常量表达式`，`a`可用于需要常量表达式的地方，`constexpr`导致顶层`const`
* 常量表达式可以是算术类型、指针或引用，但指针/引用必须指向/绑定到固定地址
（引用参数实际传递变量地址）
### 别名
```c++
typedef int i_t, *p_t; // 定义了int和int*的别名
const p_t p; // int *const p;
const p_t *pp; // int *const *pp;
using integer = int;
```
### auto
* `auto [修饰符1]变量1 = 初值1[, ...];`
* 初值应具有相同基本类型
* `auto`一般忽略初值的引用类型和顶层`const`属性
* `auto &`保留初值的顶层`const`属性
* `const auto`声明顶层`const`
```c++
const int ci = 0;
auto &r1 = ci; // const int &r1 = ci;
// auto &r2 = 0; // 错误：0推断为int，但int&不可绑定常量，需要const auto &r2 = 0;
```
### decltype
* `decltype(表达式) [修饰符1]变量1 = 初值1,[ ...];`推断类型但不求值
* `decltype(变量)`保留变量的引用类型和顶层`const`，这是使用引用不作为变量别名的唯一场景
* `decltype(非变量的表达式)`若为左值则为引用类型
```c++
const int a = 0, &r = a, *p = &a;
decltype(a) b = 0; // const int b = 0;
decltype(r) c = a; // const int &c = a;
decltype((a)) d = a; // const int &d = a;
decltype(*p) e = a; // const int &e = a;
```
* `=`赋值表达式返回左值
### struct
* 类内初始化可以使用`=`或`{}`，不可使用`()`
## 3
### using
* `using std::cin;`
* 头文件不应包含`using`
### string构造
```c++
string s1; // ""
string s2(s1); // 复制s1
string s2 = s1; // 等价
string s3("a"); // 复制字符串常量，不包括'\0'
string s3 = "a"; // 等价
string s4(3, 'a'); // "aaa"
string s4 = string(3, 'a'); // 等价
```
### string操作
```c++
os << s
is >> s // 非空白字符串
getline(is, s) // 不保存'\n'，返回is
s.empty()
string::size_type s.size() // 字节数
s[n] // n >=0 && n < s.size()，否则结果未定义
s1 + s2
s1 = s2
s1 == s2
!=, <, <=, >, >=
```
* 使用`cname`版本的C库（函数位于`std`），而不是`name.h`
```c++
for (auto c : s) // 复制
for (auto &c : s) // 可修改
for (decltype(s.size()) i = 0; i != s.size(); ++i)
```
