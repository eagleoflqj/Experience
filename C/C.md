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
# 6
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
