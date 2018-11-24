# 安装
```sh
pip install wordcloud
```
# 使用
```python
from wordcloud import WordCloud
# 实例化
wc=WordCloud(width=宽,height=高,max_words=最多词数,collocations=False)
# 生成
wc.generate(空格隔开的文本)
# 展示
import matplotlib.pyplot as plt
plt.imshow(wc)
plt.show(wc)
# 输出
wc.to_file(jpg文件名)
