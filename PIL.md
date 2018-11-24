## 导入
```python
from PIL import Image
```
## 加载图像
```python
image=Image.open(文件名)
```
## 图像通道
```python
image.getbands()
```
## 图像宽高
```python
image.size
```
## 图像像素点
```python
image.getpixel((x,y))
```
## 图像额外信息
```python
image.info
```
## 图像缩放
```python
new_image=image.resize((new_x,new_y))
```
## 图像剪切
```python
new_image=image.crop((min_x,min_y,max_x,max_y))
```
## 提取图像矩阵
```python
import numpy as np
matrix=np.array(image)
```
## 图像矩阵转图像
```python
image=Image.fromarray(matrix)
```
## 图像保存
```python
image.save(file_name)
```
