# 安装
官网下载
# 运行
## 直接运行
```sh
java -jar jenkins.war [--httpPort=端口]
```
## 部署到tomcat
复制jenkins.war到webapps下，tomcat管理页启动
# 配置
主页->系统管理->全局工具配置
## jdk
新增jdk，取消自动安装，指定JAVA_HOME
## maven
新增maven，取消自动安装，指定MAVEN_HOME
