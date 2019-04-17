# 安装
先安装ruby和ruby-dev
```sh
gem install fluentd
```
# 配置
fluentd依赖插件收集日志并推送  
收集插件命名为in_源，负责将日志转换为fluentd事件，包括标签（用于分类）、时间、记录（json格式）  
推送插件命名为out_目的  
@log_level设置插件的日志级别
## 生成
```sh
fluentd -s [目录]
```
会在目录（默认为/etc/fluent）下生成默认配置文件fluent.conf
## 输入
### tcp
```xml
<source>
  @type forward
</source>
```
```sh
echo json字符串 | fluent-cat [标签]
```
### http
```xml
<source>
  @type http
  port 端口
  bind IP地址，默认0.0.0.0
</source>
```
```
http://IP地址/[标签]?json=json字符串
```
### tail
```xml
<source>
  @type tail
  path /var/log/apache2/access.log
  # refresh_interval 60
  # read_from_head false
  pos_file /var/log/td-agent/httpd-access.log.pos
  # exclude_path []
  tag apache.access
  <parse>
    @type apache2
  </parse>
</source>
```
* path可以使用strftime和*，如/%Y/%m/%d/*
* 当path不确定时，每refresh_interval秒检查是否需要监视新文件，但丢弃开始监视前的行（可以通过read_from_head更改）
* read_from_head指定为true时使没有记录偏移量的文件（包括刚开始监视的）从头读取
* pos_file指定的文件实时记录了日志偏移量，以便fluentd重启时没有重复或遗漏
* exclude_path json数组指定排除的文件
* tag可以使用占位符：path为/path/to/file且tag设为foo.*时，最终tag为foo.path.to.file
## 解析
## 过滤
链式过滤  
符号|匹配
-|-
*|1级标签
**|0或多级标签
{a,b}|a或b
#{...}|ruby表达式
### 按json键值排除
```xml
<filter [标签]>
  @type grep
  <exclude>
    key 键
    pattern 正则表达式
  </exclude>
</filter>
```
### 改变键值
```xml
<filter [标签]>
  @type record_transformer
  <record>
    键 值 # 增加键值
    ...
  </record>
  remove_keys 键,... #删除键值
</filter>
```
## 输出
若match位于filter前，则filter对该match不起作用  
一个事件被match一次后不再继续
### stdout
非缓冲式输出
```xml
<match [标签]>
  @type stdout
</match>
```
### file
```xml
<match [标签]>
  @type file
  path 存储目录
</match>
```
* 缓冲时临时文件存储在path，刷新缓冲时日志存储为path+time+".log"
## 缓冲
位于&lt;match&gt;中
```xml
<buffer [键]>
  @type file
  # timekey 秒
  # timekey_wait 600
  # flush_at_shutdown true或false
</buffer>
```
* 若不指定@type，默认为输出插件指定的||memory
* 当&lt;match&gt;的path包含${键}或strftime（至少精确到日）时，buffer需声明键或time
* timekey无默认值，需要大于等于strftime指定的最小间隔
* 当到达timekey时间节点时，等待timekey_wait秒后刷新到path中，占位符被替换
* flush_at_shutdown是否在fluentd退出时刷新缓冲，对于file默认false，memory默认true
## label
当配置增多时，顺序执行的source、filter、match将会很乱，可采用label将其组合
```xml
<source>
  ...
  @label @label名
</source>
<label @label名>
  <filter>
    ...
  </filter>
  <match>
    ...
  </match>
<label>
```
在&lt;label&gt;外的针对 含label的source 的filter将不生效
## 系统
```xml
<system>
  log_level error # fluentd日志级别
  process_name 名称 # ps -elf的CMD列显示supervisor:名称和worker:名称
</system>
```
## @include
导入其它配置
```xml
@include 相对、绝对路径或URL
```
## 使用环境变量
"#{ENV['变量名']}"  
运行时环境变量更改无效
# 启动
```sh
fluentd [-c 配置文件]
```
# 用例
## java
### fluent.conf
```xml
<source>
  @type forward
</source>
<match 前缀.**>
  ...
</match>
```
### pom.xml
```xml
<dependency>
  <groupId>org.fluentd</groupId>
  <artifactId>fluent-logger</artifactId>
  <version>0.3.3</version>
</dependency>
```
### .java
```java
import java.util.*;
import org.fluentd.logger.FluentLogger;

public class App {
    private static FluentLogger LOG = FluentLogger.getLogger(前缀);

    public static void main(String[] args) {
        Map<String, Object> data = new HashMap<String, Object>();
        data.put(键, 值);
        LOG.log(后缀, data);// 最终标签为 前缀.后缀
    }
}
```
### python
```sh
pip install fluent-logger
```
.py
```python
from fluent import sender
from fluent import event
sender.setup(前缀, host='localhost', port=24224)
event.Event(后缀, json字典)
```
