# 安装
```sh
docker pull registry
```
# 配置
* 编辑config.yml并挂载到registry的`/etc/docker/registry/config.yml`
* 对于config.yml中的如下配置项可在启动时用`-e REGISTRY_A_B=c`覆盖
```yaml
a:
  b: c
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
* 除localhost外，主机和端口必须加入daemon.json的`insecure-registries`才可push/pull
* 可用nginx反向代理实现https，配置加入`client_max_body_size 0;`
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
-v registry目录:/var/lib/registry \
registry
```
# Docker Hub镜像
* registry启动参数添加`-e REGISTRY_PROXY_REMOTEURL=https://registry-1.docker.io`
* 宿主机`/etc/docker/daemon.json`配置registry-mirrors
