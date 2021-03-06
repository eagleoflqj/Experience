# 安装
## ubuntu
```sh
apt install golang
```
## mac
```sh
brew install go
```
# 命令
## run 编译运行
```sh
go run go文件
```
## build 编译
```sh
go build [选项] go文件...
```
选项|意义
-|-
-o 文件|指定输出文件，默认`包名.a`（非main包）或第一个源文件基础名（main包）
## version 版本
```sh
go version
```
# 结构
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello World")
}
```
* 第一行必须指定文件所属包，main表示独立的可执行程序，同一目录下的包名应相同
* 使用对象需要导入包，首字母大写对象可被外部包使用
* 先执行init，再执行main
* 注释同C
# 基本类型
* bool
* int8 int16 int32 int64
* uint8 uint16 uint32 uint64
* float32 float64
* complex64 complex128
* 机器相关 int uint
* 类型转换 类型(值)
# 变量
```go
var 变量1[, ...] 类型 [= 初值1, ...]
var 变量1[, ...] = 初值1[, ...]
var (
    ...
    [...]
)
变量1[, ...] := 初值1[, ...] // 只能出现在函数内
```
* 函数外的变量为全局变量
* 无初值则为零值
* `:=`左侧必须出现未声明过的变量
* 声明的局部变量必须使用
* `_`用于丢弃值
* 基本类型及struct存值，其余存址
* 块作用域，内层屏蔽外层
# 常量
```go
const 常量1[, ...] [类型] = 初值1[, ...] // iota = 0
const (
    ... // iota = 0
    [...] // iota = 1
)
```
* 值只可使用内置函数
* 每行若无`=`，则表达式同前
* 每个`const`可关联一个`iota`，该变量在第一行为0，每行+1
# 数组
```go
var 变量 [长度]类型
var 变量 = [长度或...]类型{值0[, ...]}
var 变量 = [1维长度]...[n维长度]类型{
    {...},
    ...
}
```
# 切片
```go
var 变量 []类型 // nil, len = cap = 0
变量 := make([]类型, 长度[, 容量])
变量 := []类型{值0[, ...]}
变量 := 数组或切片[[起点]:[终点]] // 不复制元素
变量 = append(切片, 新元素1[, ...])
copy(目的切片, 源切片)
```
* 容量默认等于长度
* make将容量内的元素均初始化为0
* make([]int, 0, 0)切片!=nil，长度和容量为0
* len和cap可用于切片和数组
* 若append后长度超过容量，将复制切片到容量为2倍的空间，但不改变指向原空间的其他变量
* copy不改变目的切片的长度，目的切片超长部分保留，源切片超长部分截断
# 字典
```go
var 变量 map[键类型]值类型 // nil, len = 0
变量 := make(map[键类型]值类型)
变量 := map[键类型]值类型{键1: 值1[, ...]}
字典[键] = 值
值变量[, 存在变量] = 字典[键]
delete(字典, 键)
```
* 不可向nil存键值对
# 结构体
```go
type 结构类型 struct {
    字段1 类型1
    [...]
}
变量 := 结构类型 {值1[, ...]} // 无初值则为零值
变量 := 结构类型 {字段1: 值1[, ...]}
```
* 结构变量或结构指针都使用`.`访问字段
# 运算符
优先级|运算符
-|-
5|*  /  %  <<  >>  &  &^
4|+  -  \|  ^
3|==  !=  <  <=  >  >=
2|&&
1|\|\||
* `++`、`--`后置，且不能用于表达式
* `a &^ b`先将b取反再与a
# 指针
```go
var 变量 *类型
```
* &和*同C
* nil表示空指针
# 条件
## if
```go
if 布尔表达式 {
    ...
} [else if 布尔表达式{
    ...
}] [else {
    ...
}]
```
## switch
```go
switch 表达式 {
case 表达式1[, ...]:
    ...
    [fallthrough]
[case ...]
[default:
    ...]
}
```
* case表达式可以不是常量，但必须与switch表达式同类型
* 默认不穿越，除非指定fallthrough
```go
switch {
case 布尔表达式1[, ...]:
    ...
[...]
}
```
## select
```go
select {
case 通信语句:
    ...
[case ...]
[default:
    ...]
}
```
* 所有通信语句被求值，随机选出不阻塞的执行其子句
* 若全阻塞且有default，执行default子句
* 若全阻塞且无default，则select阻塞，直到有通信语句解除阻塞，执行其子句
# 循环
```go
for 初始化; 条件; 更新变量 {
    ...
}
for [条件] {
    ...
}
for x[, y] := range 迭代对象 {
    ...
}
```
迭代对象|x|y
-|-|-
[]|索引|值
string|索引|字符
map|键|值
chan|数据|无
* continue、break、goto与C相同
* 通道关闭时结束循环
# 函数
```go
func [函数]([参数1 类型1, ...]) [返回类型] {
    [...
    return ...]
}
func ...(...) (返回变量1, 类型1[, ...]) {
    [...]
    return
}
func (对象 [*]结构类型) 方法([...]) [...] {
    ...
}
```
* 参数不指定类型则同下一个参数（最后一个参数必须指定类型）
* 参数按值传递
* return值时必须指定返回类型
* 函数是一等公民，支持闭包
* 命名返回变量时，`return`将其当前的值返回
# 接口
```go
type 接口类型 interface {
    方法1([类型1, ...]) [返回类型]
    [...]
}
```
# 通道
```go
变量 := make(chan 类型[, 缓冲区大小])
通道 <- 值
值变量[, 通道打开变量] = <-通道
close(通道)
```
* 缓冲区大小默认为0
* 缓冲区满时发送阻塞，缓冲区空时接收阻塞
# Goroutine
```go
go 函数(参数)
```
* 在由go运行时管理的新线程运行
