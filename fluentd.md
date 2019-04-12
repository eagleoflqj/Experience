# 安装
先安装ruby和ruby-dev
```sh
gem install fluentd
```
# 配置
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
## 过滤
标签为debug.*可匹配debug的任意一级子标签，debug.**可匹配debug及其任意子标签
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
## 输出
若match位于filter前，则filter对该match不起作用
### stdout
非缓冲式输出
```xml
<match [标签]>
  @type stdout
</match>
```
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
# 启动
```sh
fluentd [-c 配置文件]
```
# 插件
fluentd依赖插件收集日志并推送  
收集插件命名为in_源，负责将日志转换为fluentd事件，包括标签（用于分类）、时间、记录（json格式）  
推送插件命名为out_目的
