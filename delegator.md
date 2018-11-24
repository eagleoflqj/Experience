# 安装
```sh
pip install delegator.py
```
# 使用
## 导入
```python
import delegator
```
## 阻塞执行
```python
c=delegator.run(命令)，返回delegator.Command对象
# PID
c.pid
# 输出
c.out
# 错误
c.err
# 返回值
c.return_code
```
## 非阻塞执行
```python
c=delegator.run(命令,block=False)
# 等待特定输出
c.expect(输出)
# 输入
c.send(输入) # 会同时发送回车
# 等待结束
c.block()
```
# 问题
不太好用  
非阻塞时登录MySQL无法使用，Vim会多一个换行  
阻塞时就是多一个同时获取返回值、错误、输出的功能，似乎可用subprocess实现
