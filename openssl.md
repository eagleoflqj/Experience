# rand
## 生成随机数
```sh
openssl rand [参数] 字节数
```
参数|意义
-|-
-格式|hex或base64，默认二进制
-rand 文件1\[...:文件n\]|指定种子文件
-out 文件|指定输出文件，否则为stdout
# prime
## 判断素数
```sh
openssl prime [-checks 次数] [-hex] 数
```
* 默认检查20次，通过即认为是素数
* `openssl prime -checks 1 341`会误判
* 输出为`十六进制 (原始输入) is [not] prime`
## 生成素数
```sh
openssl prime -generate -bits 位数 [-safe] [-checks 次数]
```
* safe表示生成安全素数（2p+1型）
# dgst
## 消息摘要
```sh
openssl dgst [参数] [文件1 ... 文件n]
openssl 方法 ...
```
参数|意义
-|-
-方法|md5、sha1、sha512等，默认sha256
-格式|binary或hex，默认hex
-c|hex格式下:分隔字节
-out 文件|指定输出文件，否则为stdout
输入文件的消息摘要按行输出（hex）或二进制拼接（binary），不指定输入则用stdin
# 证书
Windows使用二进制.cer证书，der格式  
Linux使用base64的.crt证书，pem格式
## .cer转.crt
```sh
openssl x509 -in .cer证书 -out .crt证书 -outform pem
```
