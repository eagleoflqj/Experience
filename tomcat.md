# 安装
## JDK
下载jdk，解压缩到/usr/local  
设置$JAVA_HOME为jdk主目录  
添加$JAVA_HOME/bin到$PATH  
## Tomcat
下载tomcat，解压缩到/usr/local  
设置$CATALINA_HOME为tomcat主目录  
添加$CATALINA_HOME/lib到$CLASSPATH
# 项目
## 支持servlet注解
$CATALINA_HOME/webapps/ROOT/WEB-INF/web.xml  
设置metadata-complete="false"
## 新建项目
$CATALINA_HOME/webapps/ROOT/WEB-INF  
IntelliJ建立项目
## 配置
File->Project Structure->Modules  
Paths下选Use module compile output path，设置Output path为$CATALINA_HOME/webapps/ROOT/WEB-INF/classes  
Dependencies下添加$CATALINA_HOME/lib
## 业务逻辑
```java
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
## 启动服务
```sh
$CATALINA_HOME/bin/startup.sh
```
## 停止服务
```sh
$CATALINA_HOME/bin/shutdown.sh
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
