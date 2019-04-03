# 安装
先安装JDK
## Windows 10
官网下载解压，添加环境变量MAVEN_HOME为apache-maven-版本号目录，PATH追加%MAVEN_HOME%\bin
## Ubuntu
```sh
apt install maven
```
## 检验
```sh
mvn -v
```
# 目录
## bin maven及其调试脚本
## boot 类加载器
## conf 配置
文件|描述
-|-
settings.xml|全局配置文件
## lib 库
# 约定
约定优于配置

目录|描述
-|-
${basedir}|存放pom.xml和所有的子目录
${basedir}/src/main/java|java源码
${basedir}/src/main/resources|资源，如property文件，springmvc.xml
${basedir}/src/test/java|测试类，如Junit代码
${basedir}/src/test/resources|测试用的资源
${basedir}/src/main/webapp/WEB-INF|web应用文件目录，web项目的信息，如web.xml、本地图片、jsp视图页面
${basedir}/target|打包输出目录
${basedir}/target/classes|编译输出目录
${basedir}/target/test-classes|测试编译输出目录
Test.java|Maven只会自动运行符合该命名规则的测试类
~/.m2/repository|默认的本地仓库
~/.m2/settings.xml|用户配置文件
$MAVEN_HOME/conf/
# 配置
## jdk
$MAVEN_HOME/conf/settings.xml，&lt;profiles&gt;标签内添加
```xml
<profile>
  <id>jdk-11</id>
  <activation>
    <activeByDefault>true</activeByDefault>
    <jdk>11</jdk>
  </activation>
  <properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
</profile>
```
避免mvn compile报错：“不再支持源选项 5。请使用 6 或更高版本”和警告：“Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dependent! ”
## 仓库
加快国内访问速度，&lt;mirrors&gt;标签内添加
```xml
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  <mirrorOf>central</mirrorOf>
</mirror>
```
仓库分为本地仓库和远程仓库  
当需要依赖或插件时，先在本地仓库查找，若没有则从远程仓库下载并缓存在本地仓库  
远程仓库有三种：中央仓库（ http://repo1.maven.org/maven2 ），私服（自建的内网仓库，如nexus），其他公共仓库（如阿里云）  
镜像的作用是将符合条件的对远程仓库的请求（&lt;mirrorOf&gt;指定）转发到另一个远程仓库
## 代理
&lt;proxies&gt;内添加
```xml
<proxy>
  <id>可选名称</id>
  <active>true</active>
  <protocol>http</protocol>
  <username>用户名</username>
  <password>密码</password>
  <host>代理主机</host>
  <port>端口</port>
  <nonProxyHosts>local.net|*.host.com</nonProxyHosts>
</proxy>
```
# POM
项目对象模型，xml文件
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    <!-- 公司或者组织的唯一标志，最好设为java的package名-->
    <groupId>com.公司名.组名</groupId>
    <!-- 项目的唯一ID，一个groupId下面可能多个项目，用artifactId区分 -->
    <artifactId>项目名</artifactId>
    <!-- 工程的版本号 -->
    <version>版本号</version>
    <!-- 可选的用户友好名称 -->
    <name>名称</name>
    <!-- 打包方式，默认jar -->
    <packaging>war</packaging>
</project>
```
# 父POM
POM继承自父POM，因而pom.xml无需做更多配置，此机制称为effective pom  
在pom.xml所在目录执行如下代码查看继承后的effective pom
```sh
mvn help:effective-pom
```
输出的一部分
```xml
<build>
  <!-- 源码目录 -->
  <sourceDirectory>基目录/src/main/java</sourceDirectory>
  <!-- 脚本目录 -->
  <scriptSourceDirectory>基目录/src/main/scripts</scriptSourceDirectory>
  <!-- 测试源码目录 -->
  <testSourceDirectory>基目录/src/test/java</testSourceDirectory>
  <!-- 类输出目录 -->
  <outputDirectory>基目录/target/classes</outputDirectory>
  <!-- 测试类输出目录 -->
  <testOutputDirectory>基目录/target/test-classes</testOutputDirectory>
  <resources>
    <resource>
      <!-- 资源目录 -->
      <directory>基目录/src/main/resources</directory>
    </resource>
  </resources>
  <testResources>
    <testResource>
      <!-- 测试资源目录 -->
      <directory>基目录/src/test/resources</directory>
    </testResource>
  </testResources>
  <!-- 打包目录 -->
  <directory>基目录/target</directory>
  <!-- 打包名称 -->
  <finalName>项目名-版本号</finalName>
</build>
```
# 依赖
## 声明依赖
&lt;project&gt;标签内添加
```xml
<!-- 全部依赖 -->
<dependencies>
  <!-- 依赖项 -->
  <dependency>
    <!-- URL：仓库前缀/组名/项目名/版本号 -->
    <groupId>组名</groupId>
    <artifactId>项目名</artifactId>
    <version>版本号</version>
    <!-- 作用域，默认compile，即主代码和测试代码都可用 -->
    <scope>system</scope>
    <!-- 若中央仓库没有收录，则需要添加如下行并将jar存放到相应目录 -->
    <systemPath>${basedir}/lib/jar文件</systemPath>
  </dependency>
</dependencies>
```
作用域|描述
-|-
compile|编译、测试、运行，如spring-core
test|测试，如junit
provided|编译、测试，如servlet-api运行时由web容器提供
runtime|测试、运行，如JDBC驱动实现
system|同provided，但需要&lt;systemPath&gt;指定路径
import|与&lt;dependencyManagement&gt;配合
## 查看依赖列表
```sh
mvn dependency:list
```
只分析pom.xml，不编译，下同
## 查看依赖树
```sh
mvn dependency:tree
```
## 查看依赖问题
```sh
mvn dependency:analyze
```
列出声明和使用不一致的地方，需要重新编译
# 生命周期
## clean
阶段|描述
-|-
pre-clean|执行一些需要在clean之前完成的工作
clean|移除上一次构建生成的所有文件
post-clean|执行一些需要在clean之后立刻完成的工作
```sh
mvn clean # 将执行pre-clean和clean
```
## default或build
阶段|描述
-|-
validate|检查工程配置是否正确，完成构建过程的所有必要信息是否能够获取到
initialize|初始化构建状态，例如设置属性
generate-sources|生成编译阶段需要包含的任何源码文件
process-sources|处理源代码，例如过滤任何值
generate-resources|生成工程包中需要包含的资源文件
process-resources|拷贝和处理资源文件到目的目录中，为打包阶段做准备
compile|编译工程源码
process-classes|处理编译生成的文件，例如 Java Class 字节码的加强和优化
generate-test-sources|生成编译阶段需要包含的任何测试源代码
process-test-sources|处理测试源代码，例如，过滤任何值（filter any values）
test-compile|编译测试源代码到测试目的目录
process-test-classes|处理测试代码文件编译后生成的文件
test|使用适当的单元测试框架（例如JUnit）运行测试
prepare-package|在真正打包之前，为准备打包执行任何必要的操作
package|获取编译后的代码，并按照可发布的格式进行打包，例如 JAR、WAR 或者 EAR 文件
pre-integration-test|在集成测试执行之前，执行所需的操作例如，设置所需的环境变量
integration-test|处理和部署必须的工程包到集成测试能够运行的环境中
post-integration-test|在集成测试被执行后执行必要的操作例如，清理环境
verify|运行检查操作来验证工程包是有效的，并满足质量要求
install|安装工程包到本地仓库中，该仓库可以作为本地其他工程的依赖
deploy|部署最终的工程包到远程仓库中，以共享给其他开发人员和工程
## site
阶段|描述
-|-
pre-site|执行一些需要在site之前完成的工作
site|生成项目的站点文档
post-site|执行一些需要在site之后完成的工作，并且为部署做准备
site-deploy|将生成的站点文档部署到特定的服务器上
# 快照
一般情况下，若A推送包到远程仓库时不改变版本号，则以其为依赖的项目B不会重新拉取包；A将version、B将依赖的version改为 版本号-SNAPSHOT 可使每次构建都重新拉取
# 创建项目
```sh
mvn archetype:generate
```
按提示依次输入关键信息，maven会在当前目录下新建artifactId基目录，并自动生成项目结构
# 打包
```sh
mvn package
```
## 构建uber-jar
&lt;project&gt;&lt;build&gt;&lt;plugins&gt;标签内添加
```xml
<plugin>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <!-- 指定主类 -->
        <mainClass>包名.主类名</mainClass>
      </manifest>
    </archive>
    <descriptorRefs>
      <descriptorRef>jar-with-dependencies</descriptorRef>
    </descriptorRefs>
  </configuration>
  <executions>
    <execution>
      <id>make-assembly</id>
      <phase>package</phase>
      <goals>
        <goal>assembly</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```
将同时生成finalName.jar和finalName-jar-with-dependencies.jar，后者包含了全部依赖并指定主类
# 生成文档
&lt;project&gt;标签内添加
```xml
<build>
  <pluginManagement>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-site-plugin</artifactId>
        <version>3.7</version>
      </plugin>
    </plugins>
  </pluginManagement>
</build>
<reporting>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-javadoc-plugin</artifactId>
      <version>2.10.4</version>
    </plugin>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-project-info-reports-plugin</artifactId>
      <version>2.9</version>
    </plugin>
  </plugins>
</reporting>
```
maven-site-plugin版本大于3.3以避免java.lang.NoClassDefFoundError: org/apache/maven/doxia/siterenderer/DocumentContent，&lt;reporting&gt;避免不生成html
```sh
mvn site
```
# 其他
## 系统信息
```sh
mvn help:system
```
