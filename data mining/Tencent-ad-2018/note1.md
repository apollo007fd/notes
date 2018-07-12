# 腾讯算法大赛-2018

在线下继续做，要保证在线下验证是准确的，多建立几个测试集

同时要尽量保证迭代次数别太慢；可以试着用多线程；



路径：/home/hongbo/competition/tencent_2018/fusai/



## 数据预处理

将userFeature.data转为userFeature.csv；

将train.csv拆分成9块，(9*5059966)，train_parts_1~9.csv，每个都与adFeature.csv、userFeature.csv进行merge，分别根据aid、uid字段，merge后得到的数据存为csv文件，在/fusai/data/generated/merged文件夹下；



## 验证集和训练集的切分

将train_parts_1.csv作为训练集；由于没有时间特征，可能产生数据穿越；因此多建立几个验证集(3)个：分别用merged中train_part_9.csv、train_part_8.csv、traIn_part_7.csv中前100W条数据作为验证集。

```shell
head -n 1000001 train_part_9.csv > test_part_9.csv
```



## 数据分析\特征选择

代码：/jupyter/feature_select.ipynb

```python
train.info()
```

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5059966 entries, 0 to 5059965
Data columns (total 33 columns):
aid                   int64
uid                   int64
label                 int64
advertiserId          int64
campaignId            int64
creativeId            int64
creativeSize          int64
adCategoryId          int64
productId             int64
productType           int64
LBS                   float64
age                   int64
appIdAction           object
appIdInstall          object
carrier               int64
consumptionAbility    int64
ct                    object
education             int64
gender                float64
house                 float64
interest1             object
interest2             object
interest3             object
interest4             object
interest5             object
kw1                   object
kw2                   object
kw3                   object
marriageStatus        object
os                    object
topic1                object
topic2                object
topic3                object
dtypes: float64(3), int64(14), object(16)
memory usage: 1.2+ GB
```

### 加载数据

pd.read_csv

### 填充缺失值

#### 分析categorical feature怎么处理：

只有LBS和house有缺失值，都填充为0发现很多int64的特征，其实可以用int32来表示，这样可以节省空间。

填充前1.2G，填充后948M，占用空间缩小明显。

#### 分析object feature怎么处理：





### 计算类别特征与label的相关系数

用pearson相关系数：



## lgb baseline:

先训练好一个base的lgb模型，用验证集进行评估，然后输出特征重要性：

|                                    | testset1 | testset2 | 备注 |
| ---------------------------------- | -------- | -------- | ---- |
| lgb baseline                       | 0.710628 | 0.710234 |      |
| lgb baseline 去除4个重要性最低特征 | 0.710215 | 0.709560 | 降低 |
|                                    |          |          |      |
|                                    |          |          |      |

怎么输出特征重要性？

需要映射到原来的field上

那么需要记录每个field的vector宽度，以及前后次序。

