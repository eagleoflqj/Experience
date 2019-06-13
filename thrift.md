# 安装
```sh
apt install thrift-compiler
```
# 流程
## 编写thrift脚本
thrift类型|java类型
-|-
void|void
bool|boolean
byte|byte
i16|short
i32|int
i64|long
double|double
string|String
list|ArrayList
set|HashSet
map|HashMap
```java
namespace 语言 名称空间

include "文件名.thrift"

struct 类 {
    1: 类型 字段,
    2: ...
}

service 服务 {
    返回类型 方法(1: 类型 参数, ...)
    ...
}
```
## 编译thrift脚本
```sh
thrift [参数] 文件
```
参数|意义
-|-
-r|同时编译include的文件
-gen 语言|目标语言
## 实现Iface接口
服务Handler.java
```java
package 名称空间;

import org.apache.thrift.TException;

public class 服务Handler implements 服务.Iface {

    @Override
    public 返回类型 方法(类型 参数, ...) throws TException {
        ...
    }
    ...
}
```
## 实现server端
pom.xml
```xml
<dependency>
  <groupId>org.apache.thrift</groupId>
  <artifactId>libthrift</artifactId>
  <version>0.12.0</version>
</dependency>
```
Server.java
```java
package 名称空间;

import org.apache.thrift.server.*;
import org.apache.thrift.transport.*;

public class Server {

    public static 服务Handler handler;

    public static 服务.Processor processor;

    public static void main(String []args) {
        handler = new 服务Handler();
        processor = new 服务.Processor(handler);
        try {
            TServerTransport serverTransport = new TServerSocket(端口);
            TServer server = new TSimpleServer(new TServer.Args(serverTransport).processor(processor));
            server.serve();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
## 实现client端
