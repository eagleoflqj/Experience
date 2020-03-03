## 线性回归
```python
from sklearn import linear_model
model=linear_model.LinearRegression()
model.fit(X_train,Y_train)
model.coef_
model.intercept_
model.predict(X_test)
```

## SVM分类
```python
from sklearn import svm
model=svm.SVC()
model.fit(X_train,Y_train)
model.predict(X_test)
```
## 划分测试集
```python
from sklearn.model_selection import train_test_split
X_train,X_test,Y_train,Y_test=train_test_split(X,Y,test_size=0.2,stratify=Y) # stratify=Y 按比例划分
```
## 模型保存与加载
```python
from sklearn.externals import joblib
joblib.dump(model,文件名)
model=joblib.load(文件名)
```
