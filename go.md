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
