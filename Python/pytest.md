```sh
pytest [选项] [文件/目录 ...]
```
* 默认当前目录，递归，所有名为`test_*.py`的文件，所有名为`test_*`的函数
```py
def inc(x):
    return x + 1

def test_answer():
    assert inc(3) == 5
```
