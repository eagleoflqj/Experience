# 配置
添加环境变量JAVA_HOME为jdk目录，PATH追加JAVA_HOME/bin，CLASSPATH设为.
# 命令
## java 运行程序
```sh
java 包名.类名 [main参数]
```
java从CLASSPATH加载类，所以CLASSPATH需要包含当前目录  
对于如下C.java，需要将编译生成的C.class置于 基目录/a/b/C.class
```java
package a.b;

public class C {
    public static void main(String []args) {
    }
}
```
在基目录运行
```sh
java a.b.C
```
### 显式指定classpath
```sh
java -cp 基目录或jar包 包名.类名 [main参数]
```
### 运行含主类的jar
```sh
java -jar jar包
```
主类在jar包的META-INF/MANIFEST.MF指定
```
Main-Class: 包名.主类名
```
