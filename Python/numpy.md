# numpy
```python
import numpy as np
```
## argsort 列表元素秩
```python
y = np.argsort(x)
```
y[i]是sorted(x)[i]在x中的下标
## array 创建向量/矩阵
```py
x = np.array([1, 2, 3])
x.dtype # dtype('int64')
x.shape # (3,)
```
## base_repr 整数转进制字符串
```py
np.base_repr(整数, 进制=2)
```
