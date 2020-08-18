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
* 未定义构造函数，则编译器生成合成默认构造函数：对有初值的成员变量类内初始化，对其他成员变量默认初始化
* `类() {}`与合成默认构造函数语义相同但影响类属性（`is_trival`、`is_pod`、值初始化）
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
### 容器操作
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
### 序列容器操作
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

序列容器插入|意义
-|-
void seq.push_back(v)、void seq.emplace_back(args)|尾部插入/构造，不适用forward_list
void seq.push_front(v)、void seq.emplace_front(args)|头部插入/构造，不适用vector、string
seq.insert(iter, v)、seq.emplace(iter, args)|在iter前插入/构造
seq.insert(iter, n, v)|在iter前插入n个v
seq.insert(iter, b, e)|在iter前插入b到e的元素（b、e不可为seq的迭代器）
seq.insert(iter, il)|在iter前插入初始化列表重的元素
* insert、emplace不适用forward_list
* insert、emplace返回指向插入的第一个元素的迭代器，若insert未插入任何元素则返回iter
* 插入vector、string使迭代器、元素引用/指针失效
* 插入deque除头尾的位置使迭代器、元素引用/指针失效；插入deque头尾使迭代器失效，但元素引用/指针仍有效
* emplace在容器内构造，不产生临时对象

序列容器访问元素|意义
-|-
seq.back()|最后一个元素的引用，不适用forward_list，对空容器未定义
seq.front()|第一个元素的引用，对空容器未定义
seq\[n]|下标n元素的引用，对越界未定义
seq.at(n)|下标n元素的引用，越界抛出out_of_range异常
* 下标、at只适用string、vector、deque、array

序列容器删除|意义
-|-
void seq.pop_back()|尾部删除，不适用forward_list
void seq.pop_front()|头部删除，不适用vector、string
seq.erase(iter)|删除iter所指元素，iter为end时结果未定义
seq.erase(b, e)|删除b到e的元素
* erase不适用forward_list
* erase返回指向删除的最后一个元素之后的迭代器
* 删除deque头、尾外的元素使所有迭代器、元素引用/指针失效；删除deque尾使end失效
* 删除vector、string的元素使其后的迭代器、元素引用/指针失效

forward_list操作|意义
-|-
lst.before_begin()、lst.cbefore_begin()|指向第一个元素前的迭代器，不可解引用
lst.insert_after(iter, v)、lst.insert_after(iter, n, v)、lst.insert_after(iter, b, e)、lst.insert_after(iter, il)|在iter后插入（b、e不可为lst的迭代器）
lst.emplace_after(iter, args)|在iter后构造
lst.erase_after(iter)|删除iter之后的元素，iter或++iter为end时结果未定义
lst.erase_after(b, e)|删除++b到e的元素
* insert_after、emplace_after返回指向插入的最后一个元素的迭代器，若insert未插入任何元素则返回iter，若iter为end则结果未定义
* erase_after返回指向删除的最后一个元素之后的迭代器

序列容器更改大小|意义
-|-
void seq.resize(n)|尾部缩容，或扩容并值初始化
void seq.resize(n, v)|尾部缩容，或扩容并添加v
* vector、string、deque的所有迭代器、元素引用/指针失效

vector、string容量|意义
-|-
v.capacity()|容量
v.reserve(n)|当容量不足n时扩充容量至少到n
v.shrink_to_fit()|请求但不保证将capacity减少到size，同时适用deque
* 增长策略实现相关，但仅当capacity无法容纳size时才重新分配空间
### 字符串操作
字符串构造|意义
-|-
s(ca, n)|字符数组ca的前n个字符，数组至少长n
s(str, i[, len])|字符串str从下标i开始（至多长len）的子串，i>str.size()时抛出out_of_range异常

字符串子串|意义
-|-
s.substr(i, len)|类似子串构造函数，i默认0，len默认剩余长度

字符串更改|意义
-|-
s.insert(i, args)|在位置前插入元素
s.erase(i[, len])|从i起删除（最多len个）元素
s.assign(args)|替换
s.append(args)|追加
s.replace(range, args)|替换range指代的子串
* 上述函数均返回s的引用
* args过于复杂，见文档

字符串搜索|意义
-|-
s.find(args)|第一次出现
s.rfind(args)|最后一次出现
s.find_first_of(args)|第一次出现args中任意字符
s.find_last_of(args)|最后一次出现args中任意字符
s.find_first_not_of(args)|第一次出现args外任意字符
s.find_last_not_of(args)|最后一次出现args外任意字符

搜索函数的args|意义
-|-
c, i=0|从s\[i]开始，寻找字符c
s2, i=0|从s\[i]开始，寻找字符串s2
cs, i=0|从s\[i]开始，寻找C字符串cs
ca, i, n|从s\[i]开始，寻找字符数组ca的前n个字符
* 上述函数无匹配则返回`string::npos`，即`(string::size_type)-1`

s.compare(args)|意义
-|-
s2|s和s2
i, len, s2|s.substr(i, len)和s2
i, len, s2, i2, len2|s.substr(i, len)和s2.substr(i2, len2)
cs|s和C字符串cs
i, len, cs|s.substr(i, len)和cs
i, len, ca, n|s.substr(i, len)和字符数组ca的前n个字符

数值转换|意义
-|-
to_string(v)|将浮点数或int及以上的整数转为字符串
sto{i,l,ul,ll,ull}(s, size_t \*p = 0, b = 10)|将s的头部转为整数，若p非0则将*p设为下一个字符的下标，b进制
sto{f,d,ld}(s, size_t *p = 0)|将s的头部转为浮点数
* 非法字符串抛出invalid_argument异常，超出表示范围抛出out_of_range异常
### 容器适配器
公共接口|意义
-|-
size_type|大小类型
value_type|元素类型
container_type|底层容器类型
A a;|空适配器
A a(c);|以c为底层容器的适配器
a.empty()|是否为空
a.size()|元素个数
swap(a, b)、a.swap(b)|交换元素，要求底层容器类型也相同

适配器|底层容器|默认
-|-|-
stack|vector、deque、list|deque
queue|list、deque|deque
priority_queue|vector、deque|vector
```c++
stack<string> stk(deq);
stack<int, vector<int>> stk2;
```
* 不可使用底层容器接口
* priority_queue使用元素类型的<运算符

stack操作|意义
-|-
void stk.push(v)|入栈
void stk.emplace(args)|构造并入栈
stk.top()|取栈顶元素
void stk.pop()|出栈

队列操作|意义
-|-
void q.pop()|出队列
q.front()、q.back()|取queue头、尾元素
q.top()|取priority_queue头元素
q.push(v)|入队列
q.emplace(args)|构造并入队列
## 10
* 算法使用迭代器，不使用容器操作
* 位于头文件algorithm和numeric

算法|意义
-|-
find(b, e, v)|返回第一次出现v的迭代器或e
find_if(b, e, pred)|返回第一次pred非零的迭代器或e
count(b, e, v)|返回v出现的次数
count_if(b, e, pred)|返回pred非零的次数
T acumulate(b, e, T v)|返回b到e累加到v上的结果，要求实现v+*b
bool equal(b, e, b2)|b到e是否等于从b2开始的序列，要求实现\*b==*b2，且第一个序列不短于第二个
void fill(b, e, v)|b到e都赋值为v
fill_n(b, n, v)|b开始的n个元素赋值为v，返回尾迭代器
copy(b, e, dest)|b到e复制到dest，返回尾迭代器
copy_if(b, e, dest, pred)|b到e中pred非零元素复制到dest，返回尾迭代器
remove_if(b, e, pred)|挤压b到e中pred非零元素，返回尾迭代器（其后元素值不确定）
remove_copy_if(b, e, dest, pred)|b到e中除pred非零元素外复制到dest，返回尾迭代器
void replace(b, e, v, v2)|b到e的v替换为v2
void replace_if(b, e, pred, v2)|b到e中pred非零元素替换为v2
replace_copy(b, e, dest, v, v2)|b到e的v替换为v2复制到dest，返回尾迭代器
replace_copy_if(b, e, dest, pred, v2)|b到e中pred非零元素替换为v2复制到dest，返回尾迭代器
void reverse(b, e)|反转b到e
void reverse_copy(b, e, dest)|反转b到e复制到dest
void sort(b, e[, cmp])|排序b到e，要求实现\*b<*b
void stable_sort(b, e[, cmp])|稳定排序
unique(b, e[, cmp])|挤压相邻重复元素，返回尾迭代器（其后元素值不确定），要求实现\*b==*b
unique_copy(b, e, dest)|挤压相邻重复元素复制到dest，返回尾迭代器
partition(b, e, pred)|将b到e之间pred非零的元素放在前面，返回指向第一个false的迭代器
stable_partition(b, e, pred)|稳定分割
for_each(b, e, f)|对b到e调用f
transform(b, e, dest, f)|对b到e调用f并写入dest，dest可等于b
### lambda
\[捕获列表](形参表) mutable -> 返回类型 { 函数体 }
* `(形参表)`和`->返回类型`可省略，省略返回类型时若函数体仅含一个`return`则自动推断类型，否则返回`void`（此规则为defect）
* lambda类型由编译器生成，变量赋值需用`auto`；捕获的变量作为其类型成员
* 参数无默认值
* 捕获列表适用于局部非静态变量
* 按值捕获：复制创建lambda时的变量值，而非调用时的变量值
* 若要改变按值捕获的变量，不可省略形参表，其后跟`mutable`
* 按引用捕获：lambda执行时被引用的变量必须存在（外层函数未返回），因此外层函数不能返回捕获引用的lambda

捕获列表|意义
-|-
[]|不捕获变量
\[变量列表]|`变量`按值，`&变量`按引用，`,`分隔
\[=]|函数体出现的变量按值
\[&]|函数体出现的变量按引用
\[=, 变量列表]|列表中的变量（带`&`）按引用，其余按值
\[&, 变量列表]|列表中的变量（不带`&`）按值，其余按引用
### bind
* `function`头文件定义`bind`、`placeholders`、`ref`、`cref`
```c++
using namespace std::placeholders;
void old_callable(int a, int b, int c);
void f(int d) {
    auto new_callable = bind(old_callable, _2, d, _1); // 复制d的值
    new_callable(3, 4); // old_callable(4, d, 3);
}
void print(ostream &os, char c);
void g(ostream &os) {
    auto ref_bind = bind(print, ref(os), _1); // 不可复制的对象必须传递引用
    ref_bind('x');
}
```
### insert iterator
```c++
#include <iterator>
back_insert_iterator<T> back_inserter(T c) // push_back，连续赋值后插
front_insert_iterator<T> front_inserter(T c) // push_front，连续赋值前插
insert_iterator<T> inserter(T c, iter) // insert，连续赋值后插
iter = v // 插入
*iter, ++iter, iter++ // 简单返回iter
```
### istream iterator
```c++
istream_iterator<int> int_iter(cin); // 实现允许惰性求值，即推迟到*int_iter时读取输入流
istream_iterator<int> int_eof; // 默认初始化为eof
// 版本1
vector<int> v;
while(int_iter != int_eof)
    v.push_back(*int_iter++);
// 版本2
vector<int> v(int_iter, int_eof);
```
### ostream iterator
```c++
ostream_iterator<int> iter(cout[, " "]); // 第二个参数必须为C字符串
iter = 0; // 输出
*iter, ++iter, iter++ // 简单返回iter
```
### reverse iterator
```c++
reverse_iterator<vector<int>::iterator> rb(vec.end()); // vec.rbegin()
rb.base() == vec.end()
```
### 5种迭代器类型
* input
* output
* forward
* bidirectional
* random-access
### 列表特定算法
* 不用交换元素，效率高于通用算法

list、forward_list算法|意义
-|-
lst.merge(lst2[, cmp])|归并lst2至lst（清空lst2），要求两个列表有序，使用<或cmp
lst.remove(v)、lst.remove_if(pred)|调用erase删除==v或pred非零的元素
lst.reverse()|列表反转
lst.sort(\[cmp])|列表排序，使用<或cmp
lst.unique(\[pred])|调用erase删除相邻==或pred(l, r)非零元素

lst.splice或flst.splice_after的参数|意义
-|-
p, lst2|将lst2或flst2插入lst的p前或flst的p后，清空lst2（必须为不同列表）
p, lst2, p2|将lst2的p2移动到lst的p前，或将flst2的p2后的元素移动到flst的p后（可以为相同列表）
p, lst2, b, e|将lst2的b到e移动到lst的p前或flst的p后（可以为相同列表但p必须在b、e之外）
* 上述函数均返回void
## 11
* `map`头文件定义`map`、`multimap`，`set`头文件定义`set`、`multiset`，`unorderd_*`头文件定义它们的散列版本
* 关联容器迭代器为bidirectional
* 有序关联容器的键类型必须支持<或指定比较函数，没有<关系的两个对象认为相等
```c++
set<string> stop = {"a", "an", "the"};
map<string, int> count = {{"he", 3}, {"she", 2}};
bool cmp(int a, int b);
std::multiset<int, decltype(cmp) *> s(cmp); // 第二个模板参数为函数指针
```
### pair
```c++
#include <utility>
pair<string, int> p1; // 值初始化
pair<string, int> p2{"a", 1};
auto p3 = make_pair("a", 1);
auto p4 = pair<string, int>("a", 1);
p.first
p.second
p1 op p2 // 字典序
```
### 关联容器操作
* set::key_type = set::value_type
* map::value_type = pair<const key_type, mapped_type>
* 迭代器解引用得到value_type&
* set的迭代器为const
* 关联容器一般不使用低效的通用算法，但可作为通用算法的源或目的容器

插入|意义
-|-
c.insert(v)、c.emplace(args)|插入/构造，对map和set仅当键不存在时执行插入，返回pair&lt;iterator, bool>（first指向该键关联元素，若该键已存在则second为false）；对multimap和multiset执行插入并返回指向插入元素的迭代器
c.insert(iter, v)、c.emplace(iter, args)|插入位置很可能位于iter前，对map和set返回指向该键关联元素的迭代器，对multimap和multiset返回指向插入元素的迭代器
void c.insert(b, e)|b、e为指向value_type的迭代器
void c.insert(il)|il为value_type的初始化列表

删除|意义
-|-
c.erase(key)|删除key关联的所有元素，返回删除的个数
c.erase(iter)、c.erase(b, e)|删除iter指向的/b到e的元素，返回指向删除的最后一个元素之后的迭代器

map、unorderd_map索引|意义
-|-
mapped_type& m\[key]|返回key关联的值的引用（不适用const容器），若key不存在则添加key并值初始化值
mapped_type& m.at(key)|若key存在则返回关联的值引用，否则抛出out_of_range异常

访问元素|意义
-|-
c.find(key)|返回指向key的第一个元素的迭代器或end
c.count(key)|返回key的元素个数
c.lower_bound(key)|返回指向第一个键不小于key的元素的迭代器
c.upper_bound(key)|返回指向第一个键大于key的元素的迭代器
pair&lt;iterator, iterator> c.equal_range(key)|返回key的元素范围或{end, end}
* lower_bound、upper_bound不适用unordered，可作为插入key的hint
### 无序关联容器
管理操作|意义
-|-
c.bucket_count()|当前桶个数
c.max_bucket_count()|桶个数最大值
c.bucket_size(n)|n号桶元素个数
c.bucket(key)|key所在的桶号
local_iterator|桶内迭代器类型
const_local_iterator|桶内只读迭代器类型
c.begin(n)、c.end(n)、c.cbegin(n)、c.cend(n)|n号桶内迭代器
float c.load_factor()|平均每桶元素数
c.max_load_factor()|试图维持的平均每桶元素数的上限
c.rehash(n)|重新组织，使桶个数>=n且>size/max_load_factor
c.reserve(n)|重新组织，使元素数不超过n时无需rehash
* 内置类型（包括指针）、string、智能指针可直接用作key
```c++
struct A {
    int i;
};
size_t hasher(const A &a) {
    return hash<int>()(a.i);
}
bool eq(const A &a, const A &b) {
    return a.i == b.i;
}
using A_set = unordered_set<A, decltype(hasher) *, decltype(eq) *>
A_set s(0, hasher, eq); // 若A重载了==则可省略eq
```
## 12
* `memory`头文件定义shared_ptr、unique_ptr、weak_ptr、allocator

shared_ptr、unique_ptr公共操作|意义
-|-
shared_ptr&lt;T> sp; unique_ptr&lt;T> up;|指向类型T的空指针
p|（作为条件）是否非空
*p|解引用
p.get()|返回裸指针
p.swap(q)、swap(p, q)|交换p、q中的指针
### shared_ptr
```c++
shared_ptr<int> p1 = make_shared<int>(); // 动态分配内存并值初始化
auto p2 = make_shared<string>(3, 'a');
auto p3(p2); // p3和p2指向同一个对象，引用计数+1
auto p4 = make_shared<int>(1);
shared_ptr<int> p5(new int(2)); // explicit
shared_ptr<T> p6(uniq_ptr); // 使uniq_ptr为空
shared_ptr<T> p7(raw_ptr, callable); // 销毁*raw_ptr时以callable取代delete
p1 = p4; // p1引用计数为0，自动销毁对象并释放内存
p.unique() // 引用计数是否为1
p.use_count() // 引用计数，可能较慢
p.reset() // 置空
p.reset(raw_ptr[, callable])
```
* 是否使用引用计数由实现决定
* 复制、复制构造、裸指针构造需要指针类型可转换
* 自定义deleter适用于成员使用了动态内存但无析构函数的对象（如C对象）
### 内存管理
```c++
string *ps1 = new string; // 默认初始化
string ps2 = new string(3, 'a');
int *pi = new int(); // 值初始化
vector<int> *pv = new vector<int>{1, 2, 3};
auto p = new auto(obj); // 复制构造，不可用{}
const int *cpi = new const int(3);
int *pn = new (nothrow) int; // 失败返回空指针，默认抛出bad_alloc异常
delete ps1; // 析构+释放内存，可以接受空指针
```
* `new`头文件定义nothrow
* delete非动态分配内存或重复delete是未定义行为
* 若用裸指针初始化shared_ptr，不应继续使用裸指针
* 严禁对get的指针delete或初始化其他智能指针
```c++
{
    // int *p = new int(); // 无法释放
    shared_ptr<int> p = make_shared<int>(); // 正常释放
    throw runtime_error();
}
```
### unique_ptr
```c++
unique_ptr<int> p1(new int(1));
// unique_ptr<int> p2(p1); // 不可复制构造
// p2 = p1; // 不可复制
unique_ptr<T, D> p3, p4(raw_ptr, callable);
p = nullptr // 销毁对象并置空p
p.release() // 放弃所有权，返回对象裸指针并置空p
p.reset([raw_ptr]) // 销毁对象，转而持有raw_ptr或空
```
* 不可复制的例外：函数可以返回即将销毁的unique_ptr
### weak_ptr
```c++
weak_ptr<T> p1;
weak_ptr<T> p2(sp); // 与shared_ptr共享对象但不增加引用计数
p = wp或sp // 可用weak_ptr或shared_ptr赋值
p.reset() // 置空
p.use_count() // shared_ptr数
p.expired() // use_count是否为0
p.lock() // 若expired，返回空的shared_ptr，否则返回指向该对象的shared_ptr
```
### 动态数组
```c++
typedef string arr[10];
int *p = new arr; // 指针类型，不可使用范围for、begin/end
int *p1 = new int[3]; // 默认初始化
int *p2 = new int[3](); // 值初始化
int *p3 = new int[3]{1, 2}; // {1, 2, 0}；值初始化
// int *p = new int[1]{1, 2}; // 抛出bad_array_new_length异常
int *p4 = new int[0];
delete[] p; // 从后向前销毁对象
unique_ptr<int[]> up(new int[3]);
up[1] // 不可使用*、->
shared_ptr<int> sp(new int[3], [](int *p) { delete[] p; }); // C++17才可使用int[]
sp.get()[i]
```
* []和待释放指针不匹配是未定义行为
### allocator
* 按需构造；避免重复赋值；适用无默认构造函数的类
```c++
allocator<string> a;
auto const p = a.allocate(n); // 可存放n个string，未构造
a.construct(p + i, args) // 用args构造p[i]，其他方式使用该内存是未定义行为
a.destroy(p + i) // 析构p[i]
a.deallocate(p, n) // 释放内存，必须先析构构造的所有对象
```
* `construct`、`destroy`在C++17过时，在C++20移除

构造算法|意义
-|-
uninitialized_copy(b, e, dest)|b到e复制到未初始化的dest，返回尾迭代器
uninitialized_copy_n(b, n, dest)|复制从b开始的n个对象到未初始化的dest，返回尾迭代器
void uninitialized_fill(b, e, v)|未初始化的b到e都赋值为v
uninitialized_fill_n(b, n, v)|未初始化的b开始的n个元素赋值为v，返回尾迭代器
## 13
### 复制构造函数
* 第一个参数为该类型的引用（否则传参也需要复制构造，没有尽头），其他参数有默认值
* 第一个参数通常为`const`，函数通常不`explicit`
* 未定义复制构造函数，则编译器生成合成复制构造函数，逐成员复制：类变量调用复制构造函数，内置类型变量直接复制，数组逐元素复制（类元素复制构造）
### 复制初始化
```c++
string s1(3, 'a'); // 直接初始化
string s2(s1); // 直接初始化，调用复制构造函数
string s3 = s1; // 复制初始化
string s4 = "x"; // 隐式类型转换+复制初始化
string s5 = string(3, 'a'); // 复制初始化
```
* 复制初始化调用复制构造函数或移动构造函数
* 除使用`=`定义变量外，复制初始化还发生在传递非引用参数、返回非引用对象、列表初始化数组元素/聚合类成员
* 一些类对其分配的对象复制初始化，如初始化容器或调用insert、push，而emplace采用直接初始化
* 允许编译器将复制初始化转换为直接初始化（跳过复制/移动构造函数，但它们必须存在且可访问）
### 复制赋值运算符
* `类& operator=(const 类&)`，通常返回引用
* 未定义复制赋值运算符，则编译器生成合成复制赋值运算符：逐成员复制赋值并返回被赋值对象的引用
* 复制赋值运算符一般包含析构函数和复制构造函数的功能
### 析构函数
* `~类() {}`，先执行函数体（主要为释放动态分配的内存），再销毁成员：内置类型成员无动作，类成员调用其析构函数
* 析构函数调用时机：变量作用域结束，成员所属对象销毁，元素所属容器销毁，动态内存的指针delete，临时对象创建的语句结束
* 未定义析构函数，则编译器生成合成析构函数，与空函数体析构函数语义相同
* 分配动态内存的类需要定义析构函数，也需要定义复制构造函数、复制赋值运算符（否则多个指针值相同，错误/多次释放内存）；为避免不必要的复制开销，一般需要定义移动构造函数、移动赋值运算符
* 需要复制构造函数的类未必需要定义析构函数（如带独特序号的对象），但一般需要定义复制赋值运算符，反之亦然
### 合成函数
* 默认构造函数、复制构造函数、复制赋值运算符、析构函数可用`=default`显式让编译器生成合成函数，类外`=default`则不内联
* 任何函数可用`=delete`让编译器不定义该函数，必须在类内，一般用于禁止复制对象
* 若析构函数`=delete`，则只能动态构造对象，且不能销毁
* 若类有成员的析构函数`=delete`或不可访问，则合成析构函数`=delete`
* 若类有成员的复制构造函数或析构函数`=delete`或不可访问，则合成复制构造函数`=delete`
* 若类有成员的复制赋值运算符`=delete`或不可访问，或有成员为`const`或引用，则合成复制赋值运算符`=delete`
* 若类有成员的析构函数`=delete`或不可访问，或有成员为引用但没有类内初值，或有成员为`const`但没有默认构造函数且没有类内初值，则合成复制构造函数`=delete`
* C++11前通过声明函数为`private`但不提供定义，可达到`=delete`效果（类内、友元使用该函数产生链接错误）
### 深拷贝
```c++
class A {
    string *p;
public:
    A(const string &s = string()): p(new string(s)) {}
    A(const A &a): p(new string(*a.p)) {}
    A& operator=(const A &a) {
        auto temp = new string(*a.p); // 处理a=a的情况，且不在异常发生前修改this
        delete p;
        p = temp;
        return *this;
    }
    ~A() {
        delete p;
    }
    friend void swap(A &a, A &b);
    A(A &&a) noexcept;
    A& operator=(A &&a) noexcept;
};
inline void swap(A &a, A &b) { // 避免3次深拷贝
    using std::swap; // 后续swap调用自动选择库/自定义版本
    swap(a.p, b.p);
}
```
### 浅拷贝（引用计数）
```c++
class A {
    string *p;
    size_t *count;
public:
    A(const string &s = string()): p(new string(s)), count(new size_t(1)) {}
    A(const A &a): p(a.p), count(a.count) { ++*count; }
    A& operator=(const A &a) {
        ++*a.count;
        if(--*count == 0) {
            delete p;
            delete count;
        }
        p = a.p;
        count = a.count;
        return *this;
    }
    ~A() {
        if(--*count == 0) {
            delete p;
            delete count;
        }
    }
};
```
### swap实现复制/移动赋值运算符
```c++
A& operator=(A a) { // 复制/移动构造，自动解决a=a和异常问题
    swap(*this, a);
    return *this;
}
```
### 右值引用
```c++
int i = 0;
int &&r1 = i * 1; // 右值
// int &&r2 = i; // 错误：不可绑定左值
// int &&r3 = r1; // 错误：右值引用变量也是左值
int &&r4 = move(i); // 左值转为右值，之后不可再使用i的值
```
* 引用的对象是临时对象，没有其他使用者，即将被销毁
* 头文件`utility`定义`move`，转移对象持有的动态内存并保证对象可安全销毁
* 右值引用变量若要作为右值传递给其他函数，需要使用`move`
* 传递右值参数（非const T&&）时，f(T)和f(const T&)冲突，f(T)和f(T&&)冲突，f(const T&)和f(T&&)匹配后者
### 移动构造函数
* 第一个参数为该类型的右值引用，其他参数有默认值
```c++
// 接深拷贝A
A::A(A &&a) noexcept : p(a.p) {
    a.p = nullptr; // 保证被移动对象可销毁、值有效
}
```
* 容器操作保证异常发生时容器内容不变，如`vector`重新分配内存失败不影响原先内容；若要在重新分配时移动而非复制元素，元素类型的移动构造函数必须为`noexcept`
### 移动赋值运算符
```c++
// 接深拷贝A
A& A::operator=(A &&a) noexcept {
    if(this != &a) { // a有可能通过move得到
        delete p;
        p = a.p;
        a.p = nullptr;
    }
    return *this;
}
```
### 移动与复制
* 若未定义复制构造函数、复制赋值运算符和析构函数，且成员均可移动构造/赋值，则编译器生成合成移动构造函数/合成移动赋值运算符
* 若定义了移动构造函数/移动赋值运算符，则合成复制构造函数/合成复制赋值运算符`=delete`
* 编译器默认不生成`=delete`合成移动函数，除非显式指定移动函数为`=default`且有成员无法移动（见下）
* 若类有成员的移动构造函数或析构函数`=delete`或不可访问，则合成移动构造函数`=delete`
* 若类有成员的移动赋值运算符`=delete`或不可访问，或有成员为`const`或引用，则合成移动赋值运算符`=delete`
* 复制、移动函数同时存在时，左值匹配复制函数，右值匹配移动函数；只有复制函数时一律复制（包括使用`move`）
### 移动迭代器
```c++
move_iterator<T> make_move_iterator(T iter)
```
* 解引用返回右值引用，可用于`uninitialized_copy`实现移动语义
* 标准库不保证哪个算法可以使用移动迭代器
### 左/右值引用成员函数
```c++
class A {
    void f() &; // 左值可调用
    void g() &&; // 右值可调用
    void h() const &; // const必须出现在左边
}
void A::f() & {} // 声明和定义的&必须一致
```
* 引用成员函数和参数相同的普通成员函数冲突
## 14
* 除`()`外，运算符函数不能有默认参数
* 运算符函数作为成员函数时，`this`为第一个运算数，参数比正常少一个，显式调用：`obj1.operator+(obj2)`
* 运算符函数不作为成员函数时，至少有一个参数为类类型，显式调用：`operator+(obj1, obj2)`
* 运算符重载保持优先级和结合律
* 运算符重载不保持求值顺序、短路求值特性，`&&`、`||`、`,`（以及有默认语义的`&`）不应重载
* `::`、`.*`、`.`、`?:`不能重载
* `=`、`[]`、`()`、`->`必须作为成员函数
* 复合赋值、自增自减、解引用应作为成员函数
* 算术、关系等二元运算符不应作为成员函数
### 输入输出
```c++
ostream& operator<<(ostream&, const T&);
istream& operator>>(istream&, T&);
```
* 由于要读写私有成员，一般为类的友元函数
* 输入函数一般要处理读取失败
### 算术/关系
```c++
T operator+(const T &a, const T &b) {
    T temp = a;
    temp += b;
    return temp;
}
```
* 若用`+`实现`+=`，将产生不必要的临时对象
* 若实现了`==`，则关系运算符应与`==`协调
### 赋值
```c++
conatiner<T>& conatiner<T>::operator=(initializer_list<T>);
T& T::operator+=(const T &a);
```
### 下标
```c++
T& container<T>::operator[](size_t);
const T& container<T>::operator[](size_t) const;
```
### 自增
```c++
T& T::operator++(); // 前置
T T::operator++(int) { // 后置，int参数仅用于区分
    T temp = *this;
    ++*this;
    return temp;
}
obj.operator++(); // 前置
obj.operator++(0); // 后置
```
### 访问成员
```c++
T& iterator<T>::operator*() const;
T* iterator<T>::operator->() const {
    return &this->operator*();
}
```
* `->`的返回值必须是指针或重载了`->`的对象，后者将自动继续调用`->`直到返回指针，再访问成员
### 函数调用
```c++
return_t T::operator()(args);
```
* lambda本质为函数对象，默认`()`为`const`，除非声明为`mutable`
### 函数对象
```c++
#include <functional>
logical_and<int> intAnd; // bool operator()(int, int)
intAnd(1, 2) // 1 && 2
```
算术|关系|逻辑
-|-|-
plus|equal_to|logical_and
minus|not_equal_to|logical_or
multiplies|greater|logical_not
divides|greater_equal
modulus|less
negate|less_equal
* 接受一个模板参数
* 无关指针的比较未定义，但`sort(b, e, less<T*>())`可以按值排序指针
* 有序关联容器默认使用`less<T>`
### function
```c++
function<int(int, int)> f1 = [](int a, int b) { return a + b; } // 函数指针、functor、lambda
int (*fp)(int, int) = add;
function<int(int, int)> f2(fp); // 若add有重载函数，不能直接使用函数名
function<int(int)> f3, f4(nullptr); // 空函数，不可调用
f3 // 是否为空
f1(1, 2)
function<>::result_type
function<>::argument_type // 一元函数参数类型
function<>::first_argument_type // 二元函数参数类型
function<>::second_argument_type
```
### 类型转换
```c++
class Integer {
    int i;
public:
    Integer(int i = 0) : i(i) {}
    operator int() const { return i; }
};
Integer i = 2;
int j = i + 3;
```
* `explicit`禁用大多数隐式类型转换，但可用`static_cast`显式类型转换
* `explicit`的例外：`if`/循环/`?:`的条件，逻辑运算符的运算数
* 不应同时定义`A::A(const B&)`和`B::operator A()`
* 不应从多种算术类型转换，也不应转换为多种算术类型
```c++
void f(A);
void f(B);
// f(0) // 不是同一类型转换，A::A(int)不比B::B(double)优先
void g(C);
g((short)0) // 同一类型转换short->C，比较转换函数之前或之后的标准转换，C::C(int)比C::C(double)优先
```
## 15
```c++
struct Base {
    Base() {}
    Base(const Base &b) {}
    Base(Base &&b) noexcept {}
    Base& operator=(const Base &b) { return *this; }
    Base& operator=(Base &&b) noexcept { return *this; }
    virtual ~Base() = default;

    void f() {}
    virtual void vf() const {}
    static void sf() {}
};
struct Derived; // 派生类声明不包括派生列表
struct Derived : public Base { // 基类必须已定义
    Derived() {}
    Derived(const Derived &d) : Base(d) {} // 省略Base(d)则基类默认初始化
    Derived(Derived &&d) noexcept : Base(move(d)) {}
    Derived& operator=(const Derived &d) {
        Base::operator=(d);
        return *this;
    }
    Derived& operator=(Derived &&d) {
        Base::operator=(move(d));
        return *this;
    }

    auto vf() const -> void override final { // 不可被重写
        Derived::sf(); // Base::sf()
        sf();
    }
};
class Last final : Derived {}; // 不可被继承
```
### 继承
* 派生类指针/对象可以自动转换为基类指针/引用，但反之不行
* 若基类至少有一个虚函数，可用`dynamic_cast`将基类指针/引用转为派生类指针/引用，运行时检查；已知该转换合法时，可用`static_cast`
* `基类(派生类对象)`为复制构造，只涉及派生类中属于基类的部分
* 派生类的成员屏蔽基类的同名成员，访问基类同名成员需加`基类::`
* 派生类重写基类的部分重载函数，可用`using`取消对未重写的重载函数的屏蔽
### 虚函数
* 除构造函数外的非静态成员函数，类内声明为`virtual`则为虚函数，通过指针/引用调用时动态绑定；非虚函数或不通过指针/引用调用的虚函数编译时绑定
* 基类声明的虚函数，在派生类中默认也为虚函数
* `override`显式声明该函数为重写的虚函数
* 不重写函数则默认基类版本
* 重写函数参数必须与基类相同，返回值可以不同，但仅限有继承关系的指针/引用
* 指针、引用的静态（编译时）/动态（运行时）类型可以不同，其他表达式的静态/动态类型相同
* 若虚函数使用默认参数，则默认值为静态调用时的默认值，因此基类和派生类的虚函数的参数默认值应相同
* `基类指针->基类::虚函数()`强制编译时绑定，一般仅用于成员函数/友元函数中
* 虚函数声明为`=0`则为纯虚函数，含纯虚函数的类或继承自抽象类但未重写纯虚函数的类为抽象类，不可实例化，但构造函数仍被派生类调用
### 访问控制
* `protected`成员允许派生类及其友元访问，禁止其他类访问
* 派生类定义为`class`则默认`private`继承，定义为`struct`则默认`public`继承
* 继承方式决定了用户代码访问派生类继承自基类的成员的权限，派生类及其友元不受继承方式限制
* 只有`public`继承允许用户代码在使用基类的地方使用派生类
* `protected`继承允许派生类的派生类及其友元访问基类的`public`和`protected`成员
* 友元类的派生类不是友元类
* 派生类访问控制符后的`using 基类::成员;`使得基类的成员（要求派生类可以访问该成员）作为派生类的成员服从访问控制符（而非服从基类的访问控制符或继承方式）
### 构造/复制/析构
* 基类先于派生类构造，初始化列表可以调用基类构造函数，不指定则调用基类的默认构造函数
* 派生类显式定义的构造函数、赋值运算符需要显式调用基类相关函数
* `using 基类::基类`继承基类除默认、复制、移动构造函数外的其他构造函数，派生类的成员默认初始化，该构造函数可访问性、`explicit`、`constexpr`与其在基类中的性质相同；对有默认值的基类构造函数，派生类继承多个不带默认值的构造函数；派生类定义的构造函数覆盖继承的相同参数的构造函数；若派生类只有继承的构造函数（包括相同参数覆盖），则编译器生成合成默认构造函数
* 派生类的析构函数只负责销毁自己的资源，完成后自动调用基类的析构函数
* 基类的析构函数应为虚函数，以保证动态分配对象的正常释放，此时编译器不生成合成移动构造函数/合成移动赋值运算符，从而派生类也不生成；若要派生类生成合成移动函数，基类可以定义移动函数`=default`（从而也要定义复制函数）
* 构造函数/析构函数中调用虚函数，动态类型与构造/析构函数所在类型相同
* 若基类的默认构造函数/复制构造函数/复制赋值运算符/析构函数`=delete`或不可访问，则派生类的相应合成函数`=delete`
* 若基类的析构函数`=delete`或不可访问，则派生类的合成默认、复制构造函数`=delete`
* 若基类的移动构造函数/移动赋值运算符`=delete`或不可访问，则派生类不生成合成移动构造函数/移动赋值运算符，`=default`则生成`=delete`
### 容器
* 在容器中使用继承，应存放（智能）指针，基类智能指针可以自动转换为派生类智能指针
* 为向容器插入基类（智能）指针，可以在基类、派生类定义虚函数clone返回指向动态分配对象的指针
## 16
### 函数模板
```c++
template <typename T>
int compare(const T &a, const T &b) {
    if(less<T>()(a, b)) return -1;
    if(less<T>()(b, a)) return 1;
    return 0;
}
compare(1, 0) // T = int

template <size_t N>
size_t len(const char (&s)[N])
{
    return N - 1;
}
len("123"); // 3
```
* `typename`可换成`class`
* 非类型模板参数必须为常量表达式
* `inline`、`constexpr`可出现在模板参数列表后
* 编译器不对模板定义生成代码，而在模板实例化时生成代码
* 一般在头文件定义类、声明（成员）函数，但模板类的成员函数、模板函数应在头文件定义
### 类模板
```c++
template <typename> struct C; // 限定友元的模板参数时必须提前声明
template <typename> struct Blob;
template <typename T> bool operator==(const Blob<T>&, const Blob<T>&);
template <typename T>
struct Blob {
    Blob(); // 类作用域内使用类名不需要带模板参数
    friend T; // T是Blob<T>的友元
    friend class A; // A是Blob所有实例的友元
    template <typename X> friend class B; // B的所有实例是Blob所有实例的友元
    friend class C<T>; // C<T>是Blob<T>的友元
    friend bool operator==<T>(const Blob<T>&, const Blob<T>&); // operator==<T>是Blob<T>的友元
};
template <typename T>
Blob<T>::Blob() {}
```
* 成员函数按需实例化
* 静态成员属于模板实例
* `unique_ptr`的类模板包括deleter类型，无运行时开销；`shared_ptr`的deleter运行时需要间接访问
### 模板类型别名
```c++
template <typename T> using twin = pair<T, T>;
twin<string> name; // pair<string, string>
template <typename T> using score = pair<T, unsigned>;
score<twin<string>> midterm; // pair<twin<string>, unsigned>
```
### 模板参数
* 模板参数不能在作用域内重定义
* `含模板的类型::成员`中成员可能是静态成员或类型成员，前置`typename`表示类型成员
```c++
template <typename T, typename F = less<T>>
int compare(const T &a, const T &b, F f = F()) {
    if(f(a, b)) return -1;
    if(f(b, a)) return 1;
    return 0;
}

template <typename T = int>
struct A {};
A<> a; // A<int>
```
### 成员模板函数
```c++
template <typename T>
struct A {
    template <typename U>
    void f(U);
};
template <typename T>
template <typename U>
void A<T>::f(U) {}
```
* 不能为虚函数
* 在模板类外定义成员模板函数，要同时提供类模板和函数模板
### 模板实例化
```c++
// A.h
template <typename T>
struct A {};
template <typename T>
void f(T) {}
// a.cpp
#include "A.h"
template class A<int>; // 实例化定义，恰好出现一次
template void f(int);
// main.cpp
#include "A.h"
extern template class A<int>; // 实例化声明，不生成模板实例化代码
extern template void f(int);
```
* 解决不同编译单元对同一模板同样参数实例化，浪费时间空间的问题
* 类模板的实例化定义会实例化所有成员（此处存疑，成员模板函数不实例化），因此只适用可使用所有成员函数的类
### 模板参数推断
* 忽略顶层`const`
* 仅有的转换为非底层`const`传递给底层`const`，当参数不是引用时数组、函数转换为相应指针
* 同一模板参数在不同位置推断为不同类型时报错
* 函数名后可显式指定前几个模板参数类型，由此确定类型的参数适用普通的转换规则
* 仅用于返回值的模板类型不可推断，必须显式指定
* 可用后置返回类型和`decltype`从函数参数推断返回类型
```c++
template <typename T>
void f(T) {}
void (*fp)(int) = f; // f<int>
void g(void (*fp)(int)) {}
void g(void (*fp)(double)) {}
// g(f); // 错误，多个匹配
```
### 类型转换模板
Mod|T|Mod&lt;T>::type
-|-|-
remove_reference|X&，X&&，其他|X，X，T
add_lvalue_reference|X&，X&&，其他|T，X&，T&|
add_rvalue_reference|X&、X&&，其他|T，T&&
remove_const|const X，其他|X，T
add_const|X&、const X、函数，其他|T，const T
remove_pointer|X*，其他|X，T
add_pointer|X&、X&&，其他|X*，T*
make_signed|unsigend X，其他|X，T
make_unsigend|signed类型，其他|unsigned T，T
remove_extent|X\[n]，其他|X，T
remove_all_extent|X\[n1]\[n2]...，其他|X，T
### 模板参数引用
* `T&`只接受左值，当参数为`const`时`T`也为`const`
* `const T&`接受左值和右值，当参数为`const`时`T`不为`const`
* `T&&`接受左值和右值，接受左值时`T`为左值引用
* 间接使用引用的引用时，引用折叠：`X& &`、`X& &&`、`X&& &`等价于`X&`，`X&& &&`等价于`X&&`
* `T&&`一般用于转发或重载，当和`const T&`并存时，前者接受非`const`右值，后者接受左值和`const`右值
### move
```c++
template <typename T>
typename remove_reference<T>::type&& move(T &&t) {
    return static_cast<typename remove_reference<T>::type&&>(t);
}
```
-|move(string(""))|move(s)
-|-|-
T|string|string&
参数类型|string&&|string&
remove_reference&lt;T>::type|string|string
返回类型|string&&|string&&
static_cast|无作用|左值转右值
### 转发
* `T&&`形参保持实参的左右值和`const`属性
```c++
template <typename T>
T&& forward(typename remove_reference<T>::type &&t) {
    return static_cast<T&&>(t);
}
template <typename T>
T&& forward(typename remove_reference<T>::type &t) {
    return static_cast<T&&>(t);
}
template <typename T, typename U>
void use_forward(T &&t, U &&u) {
    pass_to_function(forward<T>(t), forward<U>(u));
}
```
-|use_forward(string(""))|use_forward(s)
-|-|-
T|string|string&
参数类型|string&&|string&
static_cast、返回类型|string&&|string&
* 完美转发将模板函数的`T&&`形参按实参属性传递给其他（按属性重载的）函数
### 重载
* 推断成功的模板函数都是候选函数、可行函数
* 若可行函数无唯一最佳匹配，在两种情况下不报错：有唯一非模板函数；没有非模板函数，但有一个模板函数比其他更特化
```c++
template <typename T>
void f(const T&) {}
template <typename T>
void f(const T*) {}
const string s;
f(&s); // const T*
```
### 变长模板
```c++
template <typename... Args> // Args模板参数包，...表示0或多个模板参数
void f(const Args &... rest) { // rest函数参数包
    cout << sizeof...(Args) << sizeof...(rest); // constexpr
}
f();
f(1.0, 2, string("")); // f(const double&, const int&, const string&)

template <typename T>
ostream& print(ostream &os, const T &t) {
    return os << t;
}
template <typename T, typename... Args>
ostream& print(ostream &os, const T &t, const Args &... rest) { // 展开Args
    os << t << ", ";
    return print(os, rest...); // 展开rest
}

template <typename T, typename... Args>
shared_ptr<T> make_shared(Args &&... args) {
    return shared_ptr<T>(new T(forward<Args>(args)...)); // 展开Args和args
}
```
### 模板特化
```c++
template <typename T>
void f(const T&) {}
template <>
void f(char *const &) {}
```
* 特化是一种覆盖默认实现的实例化而非重载，因此不影响函数匹配
```c++
class A {
    int i;
    string s;
    friend class std::hash<A>;
public:
    A(int i, string s) : i(i), s(s) {}
    bool operator==(const A &a) const {
        return i == a.i && s == a.s;
    }
};
namespace std { // 必须与模板在同一命名空间
    template <>
    struct hash<A> {
        typedef size_t result_type; // 必须定义的类型
        typedef A argument_type;
        size_t operator()(const A &a) const { // ==的对象hash值必须相同
            return hash<int>()(a.i) ^ hash<string>()(a.s);
        }
    };
}
```
### 模板偏特化
```c++
template <typename T>
struct remove_reference {
    typedef T type;
};
template <typename T>
struct remove_reference<T&> {
    typedef T type;
}
template <typename T>
struct remove_reference<T&&> {
    typedef T type;
}
```
* 只有类模板可以偏特化
```c++
template <typename T>
struct A {
    void f() {}
    void g() {}
};
template <>
void A<int>::f() {} // 偏特化可以只针对部分成员
```
## 17
### tuple
```c++
#include <tuple>
tuple<T1, ..., Tn> t; // 值初始化
tuple<T1, ..., Tn> t(v1, ..., vn); // explicit
make_tuple(v1, ..., vn) // 根据参数推断类型
==, != // 对应元素相等（必须长度相同，下同）
<, <=, >, >= // 字典序
get<i>(t) // t第i个成员的引用，左右值取决于t的左右值
tuple_size<元组类型>::value // 元组长度，constexpr
tuple_element<i, 元组类型>::type // 第i个成员的类型
```
### bitset
```c++
#include <bitset>
bitset<n> b; // n位，0，默认构造函数为constexpr
bitset<n> b(ull); // unsigned long long的低n位，n>8*sizeof(ull)则高位为0
bitset<n> b(s[, i, len, zero, one]) // 字符串s从下标i开始（长len的）子串，s[i]为b的最高位，zero和one为表示0/1的字符，遇到非法字符抛出invalid_argument异常
bitset<n> b(ca[, n, zero, one]) // 字符数组ca的前n个字符
```
bitset操作|意义
-|-
b.any()|是否有1
b.all()|是否全1
b.none()|是否全0
b.count()|1的个数
b.size()|位数，constexpr
b.test(i)|i位是否为1
b.set(i, v = true)、b.set()|i位设为v/设为全1
b.reset(i)、b.reset()|i位清0/全部清0
b.flip(i)、b.flip()|i位取反/全部取反
b\[i]|第i位
b.to_ulong()、b.to_ullong|转为unsigned long或unsigned long long，溢出抛出overflow_error异常
b.to_string(\[zero, one])|转为字符串
os << b|输出01序列
is >> b|输入01序列，遇到非01字符或达到b.size()终止
* 下标对`const`重载，非`const`时可对返回值操作，如`b[0].flip()`
* bitset支持位运算符，效果同unsigned
### 正则表达式
```c++
#include <regex>
regex r(args, flag = regex::ECMAScript); // args为string构造函数的参数
r.assign(args, flag)
r = arg // arg为string一元构造函数的参数
r.flags()
r.mark_count() // 子正则表达式数
```
flag|意义
-|-
icase|忽略大小写
nosubs|不存储子表达式匹配
optimize|优化执行速度而非构造速度
ECMAScript、basic、extended、awk、grep、egrep|正则方言
* regex运行时compile，应尽量避免创建不必要的regex
* 语法错误抛出regex_error异常，其code成员函数返回实现相关的错误码
* wregex处理wchar_t和wstring
```c++
smatch match;
regex_match(s或cs[, match], r[, mft]) // 返回是否匹配的bool，若指定match则设其为匹配
regex_search(s或cs[, match], r[, mft]) // 返回是否有子串匹配的bool
regex_replace(s或cs, r, fmt, mft = regex_constants::match_default) // 使用regex_search按fmt全部替换，返回替换后的字符串
regex_replace(dest, b, e, r, fmt[, mft]) // 替换结果写入dest指向的位置，返回尾迭代器
sregex_iterator it(b, e, r); // forward，自动调用regex_search，指向第一个smatch
sregex_iterator end; // 默认初始化为尾迭代器
```
smatch操作|意义
-|-
m.ready()|是否有效（被regex_match或regex_search设置）
m.size()|匹配失败返回0，否则返回子正则表达式数+1
m.empty()|m.size()是否为0
m.prefix()、m.suffix()|匹配部分之前/之后的ssub_match
m.format(dest, string fmt, mft = regex_constants::format_default)、m.format(dest, const char \*fmt_first, const char *fmt_last[, mft])|替换结果写入dest指向的位置，返回尾迭代器
m.format(fmt[, mft])|fmt为string或C字符串，返回替换后的字符串
m.length(n = 0)|匹配子表达式的子串长度，默认完整表达式，下同
m.position(n)|匹配的子串起始下标
m.str(n = 0)|匹配的子串
m\[n]|ssub_match
m.begin()、m.end()、m.cbegin()、m.cend()|指向ssub_match的迭代器

ssub_match操作|意义
-|-
m.matched|是否匹配了子串
m.first、m.second|子串首尾迭代器
m.length()|匹配子串长度，未匹配为0
m.str()|匹配的子串
s = m|转换为字符串，等价于s=m.str()
* s开头的类型适用于`string`匹配，c开头的类型适用`const char*`匹配

regex_constants::match_flag_type|意义
-|-
match_default|等价于format_default
match_not_bol、match_not_bow|不视第一个字符为行首/词首
match_not_eol、match_not_eow|不视最后一个字符为行尾/词尾
match_any|多于一个匹配时可返回任一匹配
match_not_null|不匹配空串
match_continuous|必须从第一个字符开始匹配
match_prev_avail|第一个字符前有字符
format_default|使用ECMAScript规则替换
format_sed|使用sed规则替换
format_no_copy|不输出未匹配的部分
format_first_only|只替换一次
### 随机数
```c++
default_random_engine e; // 默认种子
default_random_engine e(s); // 指定种子
e.seed(s) // 重设种子
default_random_engine::result_type e() // 随机非负整数
e.min() e.max() // 最小/最大随机数
e.discard(ull) // 前进ull步
uniform_int_distribution<unsigned> d(0, 9); // 包含首尾，默认int
d(e)
d.min() d.max()
d.reset() // 重设状态，使后续随机数与已生成的无关
uniform_real_distribution<> u(0, 1); // 默认double
normal_distribution<> n(0, 1); // 期望、标准差
bernoulli_distribution b(p = 0.5); // 非模板类，生成bool，参数为true概率
```
### 格式控制
控制符|意义
-|-
boolalpha、no*|以文字形式输出bool
showpos、no*|非负数先导+
hex、oct、dec|整数进制
showbase、no*|八进制先导0和十六进制先导0x
uppercase、no*|0X、ABCDEF、1E+06
fixed|不使用科学技术法，固定精度
scientific|使用科学技术法，固定精度
hexfloat|十六进制浮点数
defaultfloat|默认的依据数值大小决定是否使用科学技术法
showpoint、no*|浮点数总是输出小数点
left|左对齐
right|右对齐
internal|负号居左，数值居右
skipws、no*|跳过输入空白符，默认skipws
* 浮点数精度默认6
* `cout.precision(\[n])`：无参则返回当前精度，含参则设置精度并返回旧精度

iomanip定义接受参数的控制符|意义
-|-
setprecision(n)|设置精度
setw(n)|数值或字符串最小宽度，只适用下一项
setfill(c)|填充字符，默认空格
setbase(b)|设置进制，只接受8、10、16
### 无格式IO
单字节IO|意义
-|-
is.get(c)|读取单字节，返回is
os.put(c)|写入单字节
is.get()|读取单字节，返回int
is.putback(c)|放回单字节（应为上次读取的字节），返回is
is.unget()|回退单字节，返回is
is.peek()|查看单字节，返回int
* 保证能放回一个字节
* `cstdio`头文件定义`EOF`，可与int返回值比较

多字节IO|意义
-|-
is.get(ca, n, c)|读取最多n-1个字符存到ca，结尾补'\0'，遇到c则返回
is.getline(ca, n, c)|同上，但遇到c丢弃并返回
is.read(ca, n)|读取最多n个字节存到ca
is.gcount()|上一次无格式输入读取的字节数，若是putback、unget或peek则返回0
os.write(ca, n)|写入ca中的n个字节
is.ignore(n = 1, c = EOF)|读取并丢弃最多n个字节包括c
### 随机读写
操作|意义
-|-
tellg()、tellp()|返回当前位置，类型为pos_type
seekg(pos)、seekp(pos)|跳转到pos，pos为tellg/tellp的返回值
seekp(off, from)|跳转到相对from的位置off，from为beg、cur或end，off类型为off_type
* 上述类型、对象均为流的成员
* 随机读写应在`fstream`和`sstream`使用
* g版本适用输入流，p版本适用输出流，对输入输出流不区分读写位置，两版本等价
## 18
### 异常
* 异常发生后的栈展开过程，编译器保证退出的块中的对象正常销毁；即使异常发生在构造函数中，当前（不完整）对象也将正常销毁
* 析构函数不应抛出异常，产生的异常必须处理；栈展开时若析构函数抛出异常，程序终止
* 异常对象由`throw`的表达式复制初始化，若表达式为类对象则该类必须有可访问的析构函数和复制/移动构造函数，若表达式为数组/函数则自动转为指针
* 异常对象使用静态类型，在完全处理后销毁
* `catch`的异常对象可以为左值引用，不能为右值引用
* `catch`块或块内调用的函数可以使用`throw;`重抛当前异常，仅当异常为引用时对其的修改有效
* `catch(...)`捕获所有异常，应为最后一个`catch`，经常与重抛共同使用
```c++
struct A {};
struct B {
    A a;
    B() try : a() {
        // 构造函数体
    } catch(exception &e) {}
};
```
* `noexcept`位于`const`、`&`后，后置返回值、`final`、`override`、`=0`前
* `noexcept`的函数如果抛出异常，直接调用`terminate`，保证调用者不需处理异常
* `noexcept(true)`声明等价于`noexcept`，`noexcept(false)`声明等价于无`noexcept`
* `noexcept`运算符：`noexcept(表达式)`，表达式是否`noexcept`，返回`constexpr bool`右值，不求值表达式
* `noexcept`的函数指针只能指向`noexcept`的函数，非`noexcept`的函数指针可以指向`noexcept`的函数
* 基类的虚函数`noexcept`则派生类的重写必须`noexcept`
* 编译器自动声明合成函数、自定义的析构函数是否为`noexcept`
* 异常类定义了复制构造函数、复制赋值运算符、虚函数`what`、虚析构函数
### 命名空间
* 可以分块定义
* 命名空间内定义的模板，若要在命名空间外特化，特化必须在命名空间内声明
```c++
inline namespace name {} // inline必须出现在第一次定义
namespace name { // 自动inline
    int i;
}
void f() {
    i = 0; // name::i
}
```
* 每个文件可以定义自己的匿名命名空间，效果等同于（过时的）`static`文件作用域
* 不同文件包含同一头文件，其匿名命名空间内的名称在这些文件中分别为独立的实体
* `namespace 别名 = 命名空间;`
```c++
namespace name {
    int i = 0, j = 0, k = 0;
}
int i = 1;
void f() {
    using namespace name;
    // ++i; // 错误：::i和name::i冲突
    int j = name::i + ::i; // 屏蔽name::j
    using name::k;
    // int k; // 错误：k已在当前作用域
}
```
* `using`声明在声明时检测冲突，`using`指令在使用名称时检测冲突
```c++
operator<<(std::cout, std::string()); // 传递类对象（引用/指针）参数时，还会搜索类及其基类所在的命名空间
```
* `using`扩充当前作用域的重载函数，屏蔽外部作用域的重载函数
### 多继承
* 派生类拥有每个基类的子对象，指针赋值时地址自动改变
* 派生类构造函数调用每个基类的构造函数，调用顺序取决于基类在派生列表出现的顺序
* 派生类继承多个基类的构造函数，若其中有相同参数列表的函数，派生类必须重写该函数
* 析构函数调用顺序仍与构造函数相反，复制控制函数的合成也与单继承相同
* 每个基类在重载函数匹配中等价，可引起冲突
* 多个基类有同名成员时，无论（若是函数）参数列表是否相同、基类成员是否可见，派生类不能访问该成员，除非重写或显式指定基类
### 虚继承
* 菱形继承时，派生类包含多个间接基类子对象，解决方法：直接基类虚继承自间接基类，共享一个基类对象
```c++
struct B {};
struct B1 : virtual B {};
struct B2 : virtual B {};
struct D : B1, B2 {
    D() : B(), B1(), B2() {}
};
```
* 虚基类由最derived类负责构造，如上例构造D时依次构造B、B1、B2，B1、B2的构造函数不再调用B的构造函数
* 无论继承结构如何，虚基类先于（自己的基类外的）非虚基类构造；合成复制、移动函数的处理顺序与构造相同，析构相反
