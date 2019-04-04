# 安装
## jdk、maven 
## Tomcat
下载tomcat，解压缩到/usr/local  
设置$CATALINA_HOME为tomcat主目录  
添加$CATALINA_HOME/lib到$CLASSPATH
# 目录
## bin 启动、结束脚本
文件|描述
-|-
startup|检查环境并调用catalina
catalina|启动tomcat
shutdown|结束tomcat
## conf 配置
文件|描述
-|-
server.xml|服务器配置
tomcat-users.xml|管理员配置
## lib 库
## logs 日志
文件|描述
-|-
catalina.日期.log|控制台日志
localhost_access_log.日期.txt|用户请求tomcat的访问日志
## temp 临时
## webapps 应用
子目录名与URL对应，ROOT对应/
## work tomcat运行时编译的文件
# 配置
## web管理员
$CATALINA_HOME/conf/tomcat-users.xml的&lt;tomcat-users&gt;下添加
```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<!-- 同时支持web和命令行管理 -->
<user username="用户名" password="密码" roles="manager-gui,manager-script"/>
```
# 启动
```sh
$CATALINA_HOME/bin/startup.sh
```
# 停止
```sh
$CATALINA_HOME/bin/shutdown.sh
```
# 项目
## 创建
```sh
mvn archetype:generate
```
类型选择maven-archetype-webapp
## 目录
基目录/src/main下
### java 按包名存放servlet
### webapp jsp页面
### webapp/WEB-INF web.xml
web.xml
```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >
<!-- metadata-complete设置为false以支持servlet注解-->
<web-app
  metadata-complete="false">
  <!-- tomcat后台展示名称 -->
  <display-name>Archetype Created Web Application</display-name>
</web-app>
```
部署后webapp变为路径名并置于tomcat的webapps目录下，servlet编译生成的class位于WEB-INF/classes，作用域为compile的依赖位于WEB-INF/lib
## maven配置
pom.xml添加依赖
```xml
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
  <scope>provided</scope>
</dependency>
```
添加插件
```xml
<plugin>
  <groupId>org.apache.tomcat.maven</groupId>
  <!-- tomcat9可用7的插件，不能用tomcat-maven-plugin，否则url是写死的 -->
  <artifactId>tomcat7-maven-plugin</artifactId>
  <version>2.2</version>
  <configuration>
    <!-- 部署的文件夹与URL，/为ROOT -->
    <path>/路径</path>
    <!-- 利用tomcat的命令行管理接口部署 -->
    <url>http://localhost:8080/manager/text</url>
    <!-- 属于manager-script角色的用户 -->
    <username>用户名</username>
    <password>密码</password>
  </configuration>
</plugin>
```
## servlet
类名.java
```java
package 包名;

import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;

@WebServlet("路由")
public class 类名 extends HttpServlet {
    public void init() throws ServletException {
        // 执行必需的初始化
    }
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置解析请求的编码
        request.setCharacterEncoding("utf-8");
        // 获取请求参数
        String parameter=request.getParameter("参数名");
        // 设置响应类型
        response.setContentType("text/html;charset=utf-8");
        // 执行响应
        PrintWriter out = response.getWriter();
        out.print(响应体);
    }
    public void destroy() {
        // 执行必需的清理
    }
}
```
## 打包
```sh
mvn clean package
```
## 部署
### 初次部署
```sh
mvn tomcat7:deploy
```
### 覆盖部署
```sh
mvn tomcat7:redeploy
```
# 部署到Apache
## 安装mod-jk
```sh
apt install libapache2-mod-jk
```
## 增强安全
```sh
cd /etc/apache2/mods-enabled
```
jk.conf
```
JkOptions +RejectUnsafeURI
```
取消其注释以过滤URL中的%25和%5C
## 反向代理
根据jk.conf中
```
JkWorkersFile /etc/libapache2-mod-jk/workers.properties
```
打开上述文件，根据其中
```
worker.list=worker名
```
回到jk.conf中添加
```
JkMount Apache路由 worker名
```
修改$CATALINA_HOME/conf/server.xml中的<Engine>标签为
```xml
<Engine name="Catalina" defaultHost="localhost" jvmRoute="worker名">
```
最后需要启动Tomcat
