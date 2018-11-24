# 安装
```sh
pip install django
```
# 项目
## 新建项目
```sh
cd /var/www
django-admin startproject 项目名
cd 项目名
```
## 新建应用
```sh
python manage.py startapp 应用名
```
## 分发url到应用
项目名/urls.py
```python
from django.urls import path,include

import 应用名.urls

urlpatterns = [
    path('xxx/',include(应用名.urls))
]
```
## 建立url到函数的映射
应用名/urls.py
```python
from django.urls import path

import 应用名.views as v

urlpatterns = [
    path('yyy/',v.函数名)
]
```
## 业务逻辑
应用名/views.py
```python
from django.http import HttpResponse

def 函数名(request):
    # 处理请求
    return HttpResponse(响应体)
```
## 配置
项目名/settings.py
```python
# 允许外部访问
ALLOWED_HOSTS=['*']
# 定位静态文件
INSTALLED_APPS=[...,'应用名']
# 设置时区
TIME_ZONE='Asia/Shanghai'
USE_TZ=False
```
## 启动服务
```sh
python manage.py runserver 主机IP:端口
```
如果要监听所有可用的主机ip，则添加 0:端口  
如果不要自动加载修改过的文件，添加--noreload，这样也就没有子进程了

# 部署到Apache
## 安装mod-wsgi
```sh
apt install libapache2-mod-wsgi-py3
```
## 反向代理
```sh
cd /etc/apache2/sites-available
```
django.conf
```xml
WSGIPythonPath /var/www/项目名
WSGIScriptAlias Apache路由（没有尾部的/） /var/www/项目名/项目名/wsgi.py
<Directory /var/www/项目名/项目名>
    <Files wsgi.py>
        Require all granted
    </Files>
</Directory>
```
## 配置生效
```sh
a2ensite django.conf
service apache2 reload
```
## 设置权限
```sh
cd /var/www
chmod -R 644 项目名
find 项目名 -type d | xargs chmod 755
```
要保证每级目录的x权限  
最后无需启动django