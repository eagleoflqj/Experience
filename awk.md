默认按行处理，每行空字符分隔
# 基础示例
## 命令格式
```sh
awk [参数] '[条件] {动作}' 文件名
```
## 输出全部行
```sh
awk '{print $0}' a.txt
```
## 从控制台读取
```sh
echo '233' | awk '{print $0}'
```
## 第1项
```sh
awk '{print $1}' a.txt
```
## 被其他分隔符分开后的第2项
```sh
awk -F '分隔符' '{print $2}'
```
## 最后一项
```sh
awk '{print $NF}' a.txt
```
其中NF表示该行被分为几个字段
## 第一项和倒数第二项
```sh
awk '{print $1 $(NF-1)}' a.txt
```
## 其他内置变量
|||
|-----|-----|
|FILENAME|文件名|
|FS|字段分隔符|
|RS|行分隔符|
|OFS|输出时的字段分隔符，默认空格|
|ORS|输出时的行分隔符，默认\n|
|OFMT|数字输出格式，默认%.6g|
## 函数
字符串函数：数字自动转字符串，数学函数：字符串自动转数字
|||
|-----|-----|
|toupper|转大写|
|tolower|转小写|
|length|长度|
|substr(s,a)|从第a>0个字符开始的子串|
|substr(s,a,b)|从第a>0个字符开始的长度最多为b的子串|
|sin cos sqrt|数学函数|
|srand|随机种子，无参则当前时间|
|rand|0-1浮点随机数|
# 条件
## 包含数字的行（正则不支持\d等）
```sh
awk '/[0-9]/ {print $NF}' a.txt
```
## 奇数行
```sh
awk 'NR%2==1 {print $0}' a.txt
```
## 第一个字段等于a或b的行
```sh
awk '$1=="a"||$1=="b" {print $0}'
```
# if语句
## 第一个字段大于m的行
```sh
awk '{if($1>"m") print $0}' a.txt
```
## 第一个字段大于m的行，否则输出---
```sh
awk '{if($1>"m") print $0; else print "---"}' a.txt
```