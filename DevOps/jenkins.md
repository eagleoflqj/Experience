# 安装
官网下载
## docker
```sh
docker pull jenkins/jenkins
```
# 运行
## 直接运行
```sh
java -jar jenkins.war [--httpPort=端口]
```
## 部署到tomcat
复制jenkins.war到webapps下，tomcat管理页启动
## docker
```sh
docker run -d \
--name jenkins \
--restart always \
-v 数据目录:/var/jenkins_home \
jenkins/jenkins
```
* 数据目录需要用户1000的rwx权限
* HTTP端口8080
# 配置
主页->系统管理->全局工具配置
## jdk
新增jdk，取消自动安装，指定JAVA_HOME
## maven
新增maven，取消自动安装，指定MAVEN_HOME
# 创建任务
主页->新建任务->构建一个自由风格的软件项目
## 源码
源码管理->Git，Repository URL输入项目的git URL
## 检测更新
构建触发器->轮询SCM，日程表输入cron语法的时间  
若输入* * * * *，则每分钟jenkins会检查git是否有更新，若有更新则拉取并构建
## maven
构建->增加构建步骤->调用顶层Maven目标，目标输入maven目标，如clean package
## 单测报告
构建后操作->增加构建后操作步骤->Publish JUnit test result report，测试报告（XML）输入**/target/surefire-reports/*.xml  
工程主页可以看到最新测试结果以及趋势
## 构建产物归档
构建后操作->增加构建后操作步骤->归档成品，用于存档的文件输入**/target/*.jar或war  
工程主页可以下载最后一次成功的构建结果
## 完成
jenkins将按触发器设定查询代码仓库，当有新commit时拉取到~/.jenkins/workspace，执行构建步骤
