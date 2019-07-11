# 安装
```sh
docker pull google/cadvisor
```
# 运行
```sh
docker run -d \
-v /:/rootfs:ro \
-v /var/run:/var/run:ro \
-v /sys:/sys:ro \
-v /var/lib/docker/:/var/lib/docker:ro \
-v /dev/disk/:/dev/disk:ro \
-p HTTP端口:8080 \
--name cadvisor \
google/cadvisor
```
