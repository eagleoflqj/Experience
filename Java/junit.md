# maven配置
pom.xml添加依赖
```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>仓库中可用版本号</version>
  <scope>test</scope>
</dependency>
```
# 断言
基目录/src/test/java/多层包目录/测试名Test.java
```java
package 项目包名;

import static org.junit.Assert.assertEquals;

import org.junit.Test;

public class 测试名Test {

    @Test
    public void test单测名() {
        // 实例化
        assertEquals(预期, 结果); // 测试相等，顺序不能改，因为错误时输出会做区分
        assertArrayEquals(预期, 结果); // 测试（多维）数组相等
    }

}
```
* 每个单测单独实例化
# 运行
```sh
mvn test
```
