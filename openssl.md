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
# 证书
Windows使用二进制.cer证书，der格式  
Linux使用base64的.crt证书，pem格式
## .cer转.crt
```sh
openssl x509 -in .cer证书 -out .crt证书 -outform pem
```
