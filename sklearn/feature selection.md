# 特征选择

[官方文档](http://sklearn.apachecn.org/cn/0.19.0/modules/feature_selection.html)

## 移除低方差特征 -[`VarianceThreshold`](http://sklearn.apachecn.org/cn/0.19.0/modules/generated/sklearn.feature_selection.VarianceThreshold.html#sklearn.feature_selection.VarianceThreshold)  

假设特征是布尔值的数据集，那么就移除那些0或1的个数超过80%的特征。计算特征的方差：

Var[X] = p(1-p)

```
from sklearn.feature_selection import VarianceThreshold
X = [[0, 0, 1], [0, 1, 0], [1, 0, 0], [0, 1, 1], [0, 1, 0], [0, 1, 1]]
sel = VarianceThreshold(threshold=(.8 * (1 - .8)))
sel.fit_transform(X)
```

将会移除X的第一列，0的个数占5/6



## 单变量特征选择



## 递归式特征消除



## 使用 SelectFromModel 选取特征

### 基于 L1 的特征选取

### 基于 Tree（树）的特征选取

