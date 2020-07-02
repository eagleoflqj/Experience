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
* `decltype(表达式) [修饰符1]变量1[ = 初值1][, ...];`推断类型但不求值
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
#include <string>
string s1; // ""
string s2(s1); // 复制s1
string s2 = s1; // 等价
string s3("a"); // 复制字符串常量
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
s[n] // n >= 0 && n < s.size()，否则结果未定义
s1 + s2
s1 = s2 // 深拷贝
s1 == s2
!=, <, <=, >, >=
```
* 使用`cname`版本的C库（函数位于`std`），而不是`name.h`
```c++
for (auto c : s) // 复制
for (auto &c : s) // 可修改
for (decltype(s.size()) i = 0; i != s.size(); ++i)
```
### vector构造
```c++
#include <vector>
vector<vector<string>> vv;
vector<int> v1; // 空向量
vector<int> v2(v1);
vector<int> v2 = v1; // 等价
vector<int> v3 = {3, 1};
vector<int> v3{3, 1}; // 等价
vector<int> v4(3, 1); // {1, 1, 1}；构造初始化
vector<int> v5(2); // {0, 0}；内置类型初始化为0，实例默认初始化
vector<int> v6{2}; // {2}；尝试解释为列表初始化
vector<string> v7{2, "1"}; // {"1", "1"}；无法解释为列表初始化，尝试解释为构造初始化
vector<string> v7 = {2, "1"}; // 等价
```
### vector操作
```c++
v1.push_back(1);
v.empty()
vector<int>::size_type v1.size()
v[n]
v1 = v2 // 深拷贝
v = {1, 2} // 用元素的副本重建向量
v1 == v2 // 长度相同，对应元素相等（需要元素类型支持比较，下同）
v1 != v2
<, <=, >, >= // 字典序
```
* 为了效率不应指定向量初始大小，除非所有元素值相同
* 对向量范围for时不可增删元素
### 迭代器
* 合法迭代器指向元素或其后的位置
* 使用迭代器时不可增删元素
```c++
auto b = v.begin(), e = v.end(); // b == e等价于v空
*iter
iter->成员
++iter
--iter
iter1 == iter2
iter1 != iter2
```
### 迭代器类型
```c++
vector<int>::iterator it1;
vector<int>::const_iterator it2; // 只读
vector<int> v;
const vector<int> cv;
auto it3 = v.cbegin(), it4 = cv.begin(); // vector<int>::const_iterator
```
### vector和string的迭代器算术
```c++
iter + n
iter - n
iter += n
iter -= n
?::difference_type iter1 - iter2 // 有符号
<, <=, >, >=
```
### 二分搜索
```c++
auto beg = v.begin(), end = v.end();
auto mid = beg + (end - beg) / 2;
while (mid != end && *mid != value) {
    if (*mid < value)
        beg = mid + 1;
    else
        end = mid;
    mid = beg + (end - beg) / 2;
}
```
### 数组
* 不可用`auto`声明
* 长度必须为正的常量表达式
* 内置类型局部数组元素初值未定义，类局部数组元素默认初始化
```c++
int a[3] = {}; // {0, 0, 0}
int (*pa)[3] = &a;
int (&ra)[3] = a;
auto b(a); // int *
decltype(a) c; // int[3]
int *beg = begin(a), *last = end(a);
ptrdiff_t diff = end - beg;
```
* `size_t`、`ptrdiff_t`位于`cstddef`
* 数组可以使用范围for
* 内置类型`[]`可以接受负下标，库类型`[]`只接受非负下标
### 旧代码接口
```c++
string s;
char c_s[] = "hi";
s = c_s
s += c_s;
const char *p = s.c_str(); // 对s的后续修改可能会使p无效
strlen(p) == s.size(); // s内部有'\0'时不等
```
* C++11保证`string`连续存储且以'\0'结尾
```c++
int a[] = {0, 1, 2, 3, 4};
vector<int> v(a + 1, a + 3); // {1, 2}
```
### 高维数组
```c++
int a[3][2][1] = {{1}, {2}, {3}}; // {1, 0, 2, 0, 3, 0}
for (auto &row : a) // int (&row)[2][1]
for (auto row : a) // int (*row)[1]，下一层无法范围for
```
## 4
### 运算符
* 前置`++`、`--`返回左值，后置返回右值（类对象可能有性能损失）
* `&&`、`||`、`?:`、`,`规定求值顺序
* 求值顺序独立于优先级、结合律
* `m/n`向0取整，`m%n`和`m`同号
```c++
int i;
i = {1};
// i = {3.14}; 错误，丢失精度
i = {}; // i = 0;
string s = "s";
s = {}; // s = "";
```
* `?:`的两个结果表达式都是左值时返回左值
* 有符号数右移时符号位的处理机器相关，左移时改变符号位结果未定义
```c++
sizeof char == 1 // size_t
sizeof(int) == sizeof(int&)
int i, *p;
sizeof i
sizeof *p // 不求值
sizeof 类::字段
char a[10];
sizeof(1, a) // 10：,表达式如果右侧是左值则结果为左值
```
### 隐式类型转换
* 小整型如果正值范围包含于`int`则运算时提升为`int`，否则为`unsigned`
* `wchar_t`、`char16_t`、`char32_t`按正值范围提升到`int`、`unsigned`、`long`、`unsigned long`、`long long`、`unsigned long long`中最小的
* 小unsigned和大signed相遇，若unsigned范围包含于signed（Linux amd64的unsigned和long long）则转换为大signed，否则（Linux amd64的unsigned long和long long）都转换为大unsigned
### 显式类型转换
* `转换名<类型>(表达式)`，类型为引用时结果为左值
* `static_cast`转换数值类型，或把`void *`转成具体类型（必须实际为该类型，否则结果未定义）
* `const_cast`仅可用于消除底层`const`，但若对象本身为`const`，对其写入结果未定义；其他cast不可消除底层`const`
* `reinterpret_cast`保留底层bit，转换解释方式
* `const_cast`主要用于重载函数，慎用`reinterpret_cast`
* 旧式类型转换尝试`const_cast`或`static_cast`，若都不合法则采用`reinterpret_cast`
```c++
int i;
char *p = (char *)&i; // reinterpret_cast<char*>
```
## 5
```c++
if (int i = get_num()) { // while、switch
    // 可使用i
} else {
    // 可使用i
}
```
* `switch`非最后一个分支不可以初始化（包括默认初始化）变量，只可以声明；若初始化变量则需包含在`{}`
* `do while`条件不可定义变量，出现的变量必须在循环体外定义
* `goto`的标签和其他标识符不冲突，同`switch`一样`goto`不可跳过变量初始化到达变量作用域内
### 异常
```c++
try {
    int i;
    throw std::runtime_error("reason");
} catch (std::runtime_error err) {
    cout << err.what(); // const char *
} catch (std::exception) { // 基类异常靠后
    // 不可使用i
}
```
* 若异常最终不被捕获，自动调用库函数`terminate`（`exception`头文件定义）终止程序
* `exception`头文件定义基类`exception`
* `stdexcept`头文件定义`runtime_error`、`logic_error`及其派生类

runtime_error派生类|意义|标准库示例
-|-|-
range_error|结果超出有意义范围|`wstring_convert::from_bytes`, `wstring_convert::to_bytes`
overflow_error|结果上溢|`bitset::to_ulong`, `bitset::to_ullong`
underflow_error|结果下溢|无

logic_error派生类|意义|标准库示例
-|-|-
domain_error|参数不在定义域|无
invalid_argument|非法参数|`stoi("x")`
length_error|试图创建超长对象|`v.reserve(v.max_size() + 1)`
out_of_range|参数超限|`stoi("2147483648")`
* `new`头文件定义`bad_alloc`
* `type_info`头文件定义`bad_cast`
* `exception`、`bad_alloc`、`bad_cast`只能默认初始化（`what`返回值由编译器决定），其他异常只能用字符串或字符数组初始化
