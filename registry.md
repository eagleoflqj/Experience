# 安装
```sh
docker pull registry
```
# 运行
## HTTP
```sh
docker run -d \
-p HTTP端口:5000 \
--name registry \
--restart always \
-v registry目录:/var/lib/registry \
registry
```
* 只能通过localhost访问
## HTTPS
```sh
docker run -d \
--name registry \
--restart=always \
-v 凭证目录:/certs \
-e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
-e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/server.crt \
-e REGISTRY_HTTP_TLS_KEY=/certs/server.key \
-p HTTPS端口:443 \
registry
```
