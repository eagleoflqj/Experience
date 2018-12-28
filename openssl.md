# 证书
Windows使用二进制.cer证书，der格式  
Linux使用base64的.crt证书，pem格式
## .cer转.crt
```sh
openssh x509 -in .cer证书 -out .crt证书 -outform pem
```
