# 安装
## docker
```sh
docker pull 0xcaff/koel
```
# 启动
```sh
docker run -d \
--name koel \
-v 媒体目录:/media \
-p 本地端口:80 \
0xcaff/koel
```
# 初始化
```sh
docker exec -it koel touch database/e2e.sqlite
docker exec -it koel php artisan koel:init
```
* DB选择sqlite-e2e
* path输入`/var/www/html/database/e2e.sqlite`
* 输入用户名、邮箱、密码
* Media path输入/mediaee
# 同步媒体
```sh
docker exec -it koel php artisan koel:sync
```
# 升级
## 备份数据库
```sh
docker cp koel:/var/www/html/database/e2e.sqlite .
```
## 更新镜像及容器
## 恢复数据库
```sh
docker cp e2e.sqlite koel:/var/www/html/database
docker exec -it koel php artisan koel:init
```
