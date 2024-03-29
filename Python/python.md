# 数据结构
## dict
### 自增
```python
字典[键]=字典.get(键,0)+value
```
### 键按键排序
```python
sorted(字典)
```
### 键按值排序
```python
sorted(字典,key=字典.get)
```
# 面向对象
```py
class A:
    def __init__(self):
        self.public = 'public'
        self._protected = 'protected' # 效果同public
        self.__private = 'private'

a = A()
a.public
a._protected
# a.__private
a._A__private
```
# 转码
## ASCII、str互转
```python
chr(ascii码)
ord(字符)
```
## hex字符串、byte数组互转
```python
byte数组=bytes.fromhex(hex字符串)
hex字符串=byte数组.hex()
```
## 进制字符串转整数
```py
int(字符串, 进制=10)
```
## MD5
```python
hex字符串=hashlib.md5(byte数组).hexdigest()
```
## AES
```python
from Crypto.Cipher import AES
密文byte数组=AES.new(密钥16字节, AES.MODE_CBC, 初始向量16字节).encrypt(明文byte数组)
明文byte数组=AES.new(密钥16字节, AES.MODE_CBC, 初始向量16字节).decrypt(密文byte数组)
```
遇到无法导入_AES时
```sh
pip install pycryptodome
```
# json
## 序列化
```python
# 不进行ASCII转义，且4空格缩进
json字符串=json.dumps(对象,ensure_ascii=False,indent=4)
```
## 反序列化
```python
对象=json.loads(json字符串)
```
# logging
```py
logging.basicConfig(level=logging.NOTSET) # 否则setLevel无效
logger=logging.getLogger(__name__)
logger.setLevel(logging.INFO)
logger.info('info')
```
* DEBUG, INFO, WARNING, ERROR, CRITICAL
# os
## 工作目录
```python
os.getcwd()
```
## 修改工作目录
```python
os.chdir(目录名)
```
## 阻塞执行命令
```python
os.system(命令)
```
返回命令的返回值，0为成功
## 非阻塞执行命令
```python
stream=os.popen(命令)
''' 非阻塞代码'''
stream.read() # 阻塞，返回输出的字符串
```
## 列出目录下文件名
```python
os.listdir(目录名)
```
## 环境变量对象
```python
os.environ # 可用作字典
```
# sys
## argv 参数
```sh
python a.py 1 x # sys.argv ['a.py', '1', 'x']
```
## 脚本所在目录
```python
sys.path[0]
```
REPL下为''，windows+scrapy下为scrapy.exe
## 导包
import其他目录，先sys.path.append(目录名)
# typing
```py
from typing import List, Union, Optional

Vector = List[float]
def scale(v: Vector, s: float) -> Vector:
    return [x * s for x in v]

Union[int, str]

Optional[X] # Union[X, None]
```
# urllib
## URL拼接
相对 a.js ./a.js ../a.js /a.js  
绝对 http:// https://  
协议相关 //www
```python
from urllib.parse import urljoin
绝对URL=urljoin(当前绝对URL,目标URL)
```
# unittest
```python
import unittest

from parameterized import parameterized

class Test(unittest.TestCase):

    # 测试相等
    def equal_test(self):
        self.assertEqual(左, 右, 失败输出)

    # 参数化测试用例
    @parameterized.expand([(参数, ...), ...])
    def parameterized_test(self, ...):
        ...

if __name__ == '__main__':
    unittest.main()
```
# 读写excel
## xlrd
```python
# 若爆内存，添加ragged_rows=True非对齐加载各行
book=xlrd.open_workbook(文件名)
sheet=book.sheet_by_index(下标)
# 或sheet=book.sheet_by_name(表名)
sheet.nrows # 行数
sheet.ncols # 列数
sheet.cell_value(行,列) # 元素值
```
## xlsxwriter
```python
book=xlsxwriter.Workbook(文件名)
sheet=book.add_worksheet(表名)
sheet.write(行,列,值)
book.close()
```
# 属性
## 当前脚本可用属性
```python
dir()
```
## 对象可用属性
```python
dir(对象)
```
一般dir(类)==dir(实例)，但是对象方法用类调用需要显式指定第一个self参数  
str.startswith!=''.startswith  
bytes.fromhex==b''.fromhex
# 并行
```python
from multiprocessing import Pool
from psutil import cpu_count
if __name__=='__main__':
    pool=Pool(cpu_count(logical=False)) # 根据任务性质决定用核心数还是线程数
    result=pool.map(非lambda函数,list) # result可省
```
# 序列化
## 序列化到文件
```python
with open(文件名,'wb') as 文件:
    pickle.dump(对象,文件)
```
## 从文件反序列化
```python
with open(文件名,'rb') as 文件:
    对象=pickle.load(f)
```
lambda对象不可序列化  
函数序列化会存引用，所以重启程序或删除定义后反序列化会报错
# 问题
## UnicodeEncodeError: 'ascii' codec can't encode characters in position …
sys.stdout.encoding不是utf-8  
```sh
PYTHONIOENCODING=utf-8 python 脚本名
```
或者设置环境变量
# 其他
## jieba切词时间
jieba 111，jieba双核 57，jieba_fast 47，jieba_fast双核 25
## Windows缓存包位置
~\AppData\Local\pip\Cache\wheels\
## 继承
转到定义不存在时，可能是基类使用了派生类的成员
