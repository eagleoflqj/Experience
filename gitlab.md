# 安装
```sh
docker pull gitlab/gitlab-ce
```
# 部署
```sh
docker run -d \
-h 主机名 \
-p HTTPS端口:443 \
-p HTTP端口:80 \
-p SSH端口:22 \
--name gitlab \
--restart always \
-v config目录:/etc/gitlab:Z \
-v log目录:/var/log/gitlab:Z \
-v data目录:/var/opt/gitlab:Z \
gitlab/gitlab-ce:latest
```
* 主机名在`git remote add`中使用

容器目录|用途
-|-
/var/opt/gitlab|应用数据
/var/log/gitlab|日志
/etc/gitlab|GitLab配置
