# C++ Primer
## 1
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
* 常量表达式必须为字面类型：可以是算术类型、指针、引用、字面类，但指针/引用必须指向/绑定到固定地址
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
vector<int> v5(2); // {0, 0}；值初始化
vector<int> v6{2}; // {2}；尝试解释为列表初始化
vector<string> v7{2, "1"}; // {"1", "1"}；无法解释为列表初始化，尝试解释为构造初始化
vector<string> v7 = {2, "1"}; // 等价
```
* 值初始化：类对象若有合成默认构造函数则零初始化后默认初始化，否则默认初始化；数组对象的每个元素值初始化；其他对象零初始化
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
* 默认初始化：内置类型变量初值未定义，类变量调用默认构造函数
* 局部非静态数组，无初值则元素默认初始化
```c++
int a[3] = {}; // {0, 0, 0}；值初始化
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
string s = "s";
// i = {3.14}; // 错误，丢失精度
i = {}; // 0；类似值初始化，下同
s = {}; // ""
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
* `const_cast`仅可用于修改底层`const`，但若对象本身为`const`，对其写入结果未定义；其他cast不可修改底层`const`
* `reinterpret_cast`保留底层bit，转换解释方式
* `const_cast`主要用于重载函数，慎用`reinterpret_cast`
* 旧式类型转换尝试`const_cast`或`static_cast`，若都不合法则采用`reinterpret_cast`
```c++
int i;
char *p = (char *)&i; // reinterpret_cast<char *>
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
## 6
### 参数
* 形参可以不命名
* 局部静态变量在控制流第一次到达其定义前初始化
* 在函数内不更改的引用参数应声明为常引用，指针同理
* `argv[0]`可能为`""`
### initializer_list
```c++
#include <initializer_list>
initializer_list<int> l1; // 空列表
auto l2 = {1, 2, 3};
auto l3(l2); // 不复制元素，下同
l1 = l2
const int *beg = l1.begin(), *last = l1.end();
void f(initializer_list<string> l, int i) {
    for(auto &s : l)
        cout << s;
}
f({"1", "2", "3"}, 1);
```
### 返回值
* void函数`return`后的表达式只能为void函数的调用
* 编译器可能无法检测非void函数结尾有`return`
* 不可返回局部对象的指针（如临时的`initializer_list`）或引用
* 引用返回值是左值，其他为右值
```c++
vector<int> f() {
    return {1, 2, 3}; // 返回类对象，类决定初始化列表的解释方式
}
int g() {
    return {3}; // 不可以{3.0}
}
```
* `main`可不返回值，默认返回0
```c++
int a[3];
int (*f(int i))[3];
auto f(int i) -> int (*)[3];
decltype(a) *f(int i);
```
### 函数重载
* 不可定义仅返回值不同的重载函数
* 不区分参数顶层`const`，区分底层`const`
* 非底层`const`对象优先匹配带非底层`const`参数的函数
```c++
const string &f(const string &s) {
    return s;
}
string &f(string &s) {
    auto &r = f(const_cast<const string &>(s));
    return const_cast<string &>(r);
}
```
* 重载函数的调用可能有唯一最佳匹配、无匹配、多重匹配，后两者报错
```c++
void f(double);
void f(string);
int main() {
    void f(int); // 糟糕实践：局部声明函数
    f(3.14); // 外部声明被屏蔽，调用f(int)
}
```
### 默认参数
```c++
char g();
double h = 1;
void f(int i, double d, char c = g());
void f(int, double = h, char); // 在旧声明基础上增加了一个默认参数
int main() {
    h = 2; // 改变默认值
    int h = 3; // 屏蔽全局变量，不影响默认值
    f(0); // f(0, 2.0, g())
}
```
### 内联函数
* 返回值前加`inline`，编译器不保证展开
### constexpr函数
* 参数和返回值为字面类型，函数体恰好是一个`return`
* `constexpr`函数隐含为内联函数
* 传递非`constexpr`参数时，返回非`constexpr`值
```c++
constexpr int f(int i) { return i+1; }
int a[f(1)];
int i = 2;
// int b[f(i)]; // 错误，但有的编译器支持
```
* 内联函数、`constexpr`函数可重复定义，但定义必须一致，且编译器需要定义才能展开，因此常于头文件定义
### 调试
* `cassert`头文件定义宏`assert`，当表达式为假时终止程序
* 若定义了`NDEGUG`，则`assert`不起作用
* `__func__`为当前函数名，类型`static const char[]`
* `__FILE__`、`__LINE__`、`__TIME__`、`__DATE__`定义同C
### 函数匹配
* 候选函数：声明可见，函数名匹配
* 可行函数：候选函数中可用给定参数调用的
* 最佳匹配：相对于其他可行函数，每个参数的转换程度相同或更优，且有参数的转换程度更优
* 有唯一最佳匹配时，调用无歧义，否则报错
* 若需要对参数强制类型转换，则重载函数设计有问题
* 转换程度排序：精确匹配（相同类型、数组/函数->相应指针、只差顶层`const`），底层`const`转换，整型提升，算术/指针类型转换，类转换
### 函数指针
* 声明为函数的参数自动为函数指针
* 返回函数指针时不能声明返回值为函数
```c++
double f(int);
decltype(f) g; // 函数
decltype(f) *p; // 函数指针
```
## 7
### 成员函数
* 成员函数必须在类内声明，可在类外定义（函数名前加`类::`；若返回类型也是类成员，由于出现在函数名前，也需要加`类::`）
* 类内定义的函数隐含为内联函数
* 成员函数中`this`为指向当前对象的指针常量（顶层`const`）
* 成员函数参数表后接`const`则`this`为常量指针（底层`const`）
* 若要返回`*this`，则可根据`const`重载成员函数，非`const`对象优先调用非`const`成员函数
* 成员函数最后编译，因此可出现在其使用的成员定义之前
* 成员函数中的局部变量屏蔽同名成员变量，使用后者应用`this->`或`类::`
### 构造函数
* 构造函数不能为`const`
* 未定义构造函数，则编译器合成默认构造函数，对有初值的成员变量类内初始化，对其他成员变量默认初始化
* 显式定义合成默认构造函数：`类() = default;`
* 初始化列表：`类(...) : 成员1(...), 成员2{...} ... {...}`，初始化顺序同声明顺序（与初始化列表的顺序无关），未在初始化列表出现的成员变量类内初始化或默认初始化，所有成员变量初始化完成后构造函数体执行
* 委托构造：构造函数的初始化列表为对另一个构造函数的调用
### 访问限定
* 访问限定符可出现任意多次
* `class`和`struct`的唯一区别是第一个访问限定符前的成员声明是`private`还是`public`
### 友元
* 友元函数：类内声明，返回值前加`friend`，函数体可访问类的非`public`成员
* 友元函数不是成员函数，可在类的任意位置声明，不受访问限定符影响
* 类内声明的友元函数只负责访问控制，应在类所在头文件再次声明（一些编译器不作此要求）
* `friend class 友元类;`，友元类的所有成员函数可访问本类私有成员；友元类不可传递
* `friend 返回类型 其他类::成员函数(...);`，仅该成员函数可访问
```c++
class A;
struct B {
    void modifyA(A &);
};
class A {
    int x;
    friend void B::modifyA(A &); // 必须提前声明
    friend void f(); // 可不提前声明，下同
    friend class C;
};
void B::modifyA(A &a) {
    a.x = 0;
}
```
### 杂项
* 可用`typedef`或`using`定义类型成员，遵循访问限定，且必须先定义后使用；不可与类外的类型重名（一些编译器不作此要求）
* 成员变量声明为`mutable`，则`const`对象也可修改该成员
* 拥有相同成员的类是不同的类
* 声明类对象可以像C一样在类名前加`struct`或`class`
* 前向声明：`class 类;`，用于定义指针或引用、声明（但不是定义）以该类对象为参数或返回值的函数
* 不提供相应函数时，执行默认的复制、赋值、析构
### 隐式类型转换
* 可用单参数调用的构造函数定义了从参数类型到该类的隐式转换
* 类内声明时`explicit`禁用隐式转换（如`vector`接受元素个数参数的构造函数），类外定义不重复声明`explicit`
```c++
struct A {
    A(const string &s) {}
    explicit A(int i);
};
A::A(int i) {}
void f(const A &a) {}
int main() {
    string s;
    A a1 = s;
    f(s);
    // f(""); // 错误，隐式转换最多一次
    // f(0); // 错误
    // A a2 = 0; // 错误，必须直接初始化
    A a3 = static_cast<A>(0);
}
```
### 聚合类
* 所有成员变量都是`public`，且没有初值
* 没有定义构造函数
* 没有基类和虚函数
```c++
struct A {
    int i;
    string s;
    double d;
};
A a = {1, "a"}; // 剩余元素值初始化
```
### 字面类
* 所有成员变量都是字面类型
* 对于非聚合类还需满足：至少有一个`constexpr`构造函数；有初值的内置类型成员变量初值为`constexpr`，有初值的类成员变量使用该成员所属类的`constexpr`构造函数；使用默认的析构函数
* `constexpr`构造函数可以是`=default`或`{}`，初始化列表需包含所有无初值的成员变量且提供`constexpr`初值
### 静态成员
* 作用域外访问方式：`类::静态成员`，`对象或引用.静态成员`，`指针->静态成员`
* `static constexpr`成员变量必须类内初始化，`static const`整型成员变量可在类内初始化（初值必须为`constexpr`），其他`static`成员变量必须在类外定义
```c++
struct A {
    static int i1;
    static const int i2 = 0;
    static constexpr double d = 0;
};
int A::i1 = 0;
const int A::i2; // 由于不能将所有i2的出现替换为常量，必须在类外定义
void f(const int &i) {}
int main() {
    f(A::i2);
}
```
* `static`成员变量可以为该类类型，可以作为成员函数参数默认值
## 8
### IO类
* `iostream`头文件定义`istream`、`ostream`、`iostream`
* `fstream`头文件定义`ifstream`、`ofstream`、`fstream`
* `sstream`头文件定义`istringstream`、`ostringstream`、`stringstream`
* 上述类都有对应的`w*`类处理`wchar_t`
* `fstream`、`stringstream`等类继承自对应的`iostream`类
* IO对象不可复制、赋值
### 流状态
```c++
strm::iostate // 机器相关整数类型
strm::badbit // 不可恢复错误位
strm::failbit // 可恢复错误位
strm::eofbit // 流结束位，同时设置failbit
strm::goodbit // 0
bool s.eof() // eofbit置位则为true
bool s.fail() // failbit或badbit置位则为true，if(s)等价于if(!s.fail())
bool s.bad() // badbit置位则为true
bool s.good() // 错误位均未置位则为true
void s.clear() // 重置所有错误位
void s.clear(flags) // 设置为给定错误位
void s.setstate(flags) // 置位给定错误位
strm::iostate s.rdstate() // 返回当前状态
```
### 输出缓冲
* 刷新输出缓冲区的条件：程序正常终止、缓冲区满、显式刷新、`unitbuf`（如`cerr`）、相关的另一个流有读写行为（读取`cin`或写入`cerr`会刷新`cout`）
```c++
cout << "1" << flush; // 刷新
cout << "2" << ends; // 写空字符并刷新
cout << unitbuf; // 禁用缓冲
cout << nounitbuf; // 恢复缓冲
is.tie() // 返回指针，指向输入流绑定的输出流；输入流、输出流多对一
is.tie(&os) // 返回旧指针，绑定新输出流，读取is刷新os
```
### fstream
```c++
fstrm fs; // 未绑定文件
fstrm fs(s); // 打开文件，s为string或C字符串，失败置位failbit
fstrm fs(s, mode); // 指定模式
void fs.open(s) // 若fs已打开文件则失败
void fs.open(s, mode)
void fs.close() // fs销毁时自动调用close
bool fs.is_open()
```
mode|意义
-|-
fstrm::in|输入，ifstream、fstream默认
out|输出，ofstream、fstream默认
app|追加，与trunc冲突，隐含out
ate|立即指向文件尾，与其他模式不冲突
trunc|截断，必须同时指定out
binary|二进制模式，与其他模式不冲突
* `ofstream`不指定`in`或`app`则隐含`trunc`
### stringstream
```c++
sstrm ss; // 未绑定字符串
sstrm ss(s); // 绑定s的副本
string ss.str() // 绑定字符串的副本
void ss.str(s) // 绑定新字符串，不改变流状态
```
## 9
类型别名|意义
-|-
iterator|迭代器
const_iterator|只读迭代器
size_type|unsigned大小
difference_type|signed距离
value_type|元素
reference|value_type&
const_reference|const value_type&

构造|意义
-|-
C c|空容器（除array）
C c1(c2)|复制
C c(b, e)|迭代器范围复制（不适用array），容器类型可不同，元素类型可以转换
C c{a, b, ...}|列表初始化

赋值和交换|意义
-|-
c1 = c2|用c2的元素替换c1
c1 = {a, b, ...}|用列表元素替换c1
a.swap(b)|交换a和b的元素
swap(a, b)|同a.swap(b)

大小|意义
-|-
c.size()|元素个数（不适用forward_list）
c.max_size()|元素个数最大值
c.empty()|是否为空

插入/删除元素|意义（不适用array）
-|-
c.insert(args)|插入
c.emplace(inits)|构造并插入
c.erase(args)|删除
void c.clear()|删除全部

比较|意义
-|-
==、!=|适用全部容器
<、<=、>、>=|类似字符串比较，不适用无序关联容器

迭代器|意义
-|-
c.begin()、c.end()|iterator
c.cbegin()、c.cend()|const_iterator

反序迭代器|意义（不适用forward_list）
-|-
c.rbegin()、c.rend()|reverse_iterator
c.crbegin()、c.crend()|const_reverse_iterator

序列容器初始化|意义
-|-
seq(n)|n个值初始化元素
seq(n, v)|n个元素v
```c++
array<int, 3> a1; // 默认初始化
array<int ,3> a2 = {1, 2}; // {1, 2, 0}；值初始化
```
序列容器赋值|意义（不适用array）
-|-
seq.assign(b, e)|类似迭代器初始化，b、e不可为seq的迭代器
seq.assign(il)|用initializer_list中的元素代替
seq.assign(n, t)|n个元素v
* 赋值使被赋值容器的迭代器、元素引用/指针失效
* 交换不使容器的迭代器、元素引用/指针失效（string除外），它们都转而指向另一个容器（array除外）
