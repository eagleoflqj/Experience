# 项目
## 新建项目
```sh
scrapy startproject 项目名
```
## 修改设置
项目名/项目名/setting.py  
设置USER_AGENT  
ROBOTSTXT_OBEY = False  
取消ITEM_PIPELINES注释  
添加其他自定义配置

## 定义数据项
项目名/项目名/items.py
```python
import scrapy

class 数据项名(scrapy.Item):
    字段名=scrapy.Field()
    #多态方法，比如特定的插库语句
```
## 定义处理管道
项目名/项目名/pipelines.py
```python
class 管道名(object):#跟settings.py中的ITEM_PIPELINES一致
    def open_spider(self,spider):
        #连接数据库
    def process_item(self,item,spider):
        #根据item类型插入数据库或调用多态方法，commit
        return item
    def close_spider(self,spider):
        #关闭连接
```
## 爬取逻辑
项目名/项目名/spiders/爬虫文件名
```python
import scrapy
from 项目名.items import 数据项名
from 项目名.settings import 自定义配置名

class 爬虫类名(scrapy.Spider):
    name=爬虫名
    def start_requests(self):
        self.字段=getattr(self,参数名,默认值)#获取命令行参数
        #获取待爬urls列表
        for url in urls:
            yield scrapy.Request(url=url,callback=self.parse,meta=元信息字典)
            #可以换成response.follow，作用是省略urljoin
    def parse(self,response:scrapy.http.Response):
        #解析字段，response可用属性：body字节数组，text字符串，url，meta字典
        item=数据项名(字段名=值)
        item['字段名']=值
        yield item
    def closed(self,reason):
        #爬虫结束时的处理
        self.logger.info(信息)
```
## 启动爬虫
```sh
scrapy runspider 爬虫文件名 [–a 参数名=参数值 [-a …]]
```
或
```sh
scrapy crawl 爬虫名 [–a 参数名=参数值 [-a …]]
```
# 调试
## 查看原始渲染页面
```sh
scrapy view 地址
```
## scrapy控制台
```sh
scrapy shell 地址
```
```python
# 查看原始渲染页面
view(response)
# 尝试抽取
response.xpath("//div[@id='元素id']//tr/text()").extract_first()
```
# 注意
不要在spiders文件夹里放非spider的py文件，否则都会被当作spider被import；当然也可以加
```python
if __name__=='__main__'
```
