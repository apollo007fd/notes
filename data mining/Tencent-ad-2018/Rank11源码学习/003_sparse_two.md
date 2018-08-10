# 003_sparse_two

模块功能:

生成交叉特征. 并分别对训练集/验证集/测试集1/测试集2的交叉特征进行onehot编码

输入:

- data_preprocessing/train_test_merge.csv

输出:

- 交叉特征文件



```python
##生成组合ID特征
import pandas as pd
import lightgbm as lgb
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.preprocessing import OneHotEncoder,LabelEncoder
from scipy import sparse
import os
import numpy as np
import time
import random
import warnings
warnings.filterwarnings("ignore")
##读取数据
print("Reading...")
data = pd.read_csv('data_preprocessing/train_test_merge.csv')

print('Index...')
train_part_index = list(data[(data['label']!=-1)&(data['n_parts']!=1)].index)
evals_index = list(data[(data['label']!=-1)&(data['n_parts']==1)].index)
test1_index = list(data[data['n_parts']==6].index)
test2_index = list(data[data['n_parts']==7].index)
print('LabelEncoder...')
label_feature=['aid', 'advertiserId', 'campaignId', 'creativeId',
       'creativeSize', 'adCategoryId', 'productId', 'productType', 'age',
       'gender','education', 'consumptionAbility', 'LBS',
       'os', 'carrier', 'house']
data = data[label_feature]
for feature in label_feature:
    s = time.time()
    try:
        data[feature] = LabelEncoder().fit_transform(data[feature].apply(int))
    except:
        data[feature] = LabelEncoder().fit_transform(data[feature])
    print(feature,int(time.time()-s),'s')
print('Done')



import time
print('Combining id...')
col = ['aid', 'advertiserId', 'campaignId', 'creativeId',
       'creativeSize', 'adCategoryId', 'productId', 'productType', 'age',
       'gender','education', 'consumptionAbility', 'LBS',
       'os', 'carrier', 'house']
##组合ID编码
train_part_x_sparse=pd.DataFrame()
evals_x_sparse=pd.DataFrame()
test1_x_sparse=pd.DataFrame()
test2_x_sparse=pd.DataFrame()
n = len(col)
enc = OneHotEncoder()
num = 0
for i in range(n):  #0~15
    for j in range(n-i-1):  #0~(n-i-1) 一共120中交叉特征
        s = time.time()
        se = data[col[i]]*100000+data[col[i+j+1]]*1
        enc.fit(se.values.reshape(-1, 1))
        
        se = data.loc[train_part_index][col[i]]*100000+data.loc[train_part_index][col[i+j+1]]*1
        arr =enc.transform(se.values.reshape(-1, 1))
        train_part_x_sparse = sparse.hstack((train_part_x_sparse, arr))

        
        se = data.loc[evals_index][col[i]]*100000+data.loc[evals_index][col[i+j+1]]*1
        arr = enc.transform(se.values.reshape(-1, 1))
        evals_x_sparse = sparse.hstack((evals_x_sparse,arr))

        
        se = data.loc[test1_index][col[i]]*100000+data.loc[test1_index][col[i+j+1]]*1
        arr = enc.transform(se.values.reshape(-1, 1))
        test1_x_sparse = sparse.hstack((test1_x_sparse,arr))
        
        se = data.loc[test2_index][col[i]]*100000+data.loc[test2_index][col[i+j+1]]*1
        arr = enc.transform(se.values.reshape(-1, 1))
        test2_x_sparse = sparse.hstack((test2_x_sparse,arr))
        num+=1
        arr = []
        print(num,col[i],col[i+j+1],int(time.time()-s),"s")
        if num%12==0:
            k = num//12  #k=1~10
            ##存写组合稀疏矩阵
            print("Saving...")
            print(k)
            print('train_part_x...')
            sparse.save_npz('data_preprocessing/train_part_x_sparse_two_'+str(k)+'.npz',train_part_x_sparse)
            print('evals_x...')
            sparse.save_npz('data_preprocessing/evals_x_sparse_two_'+str(k)+'.npz',evals_x_sparse)
            print('test1_x...')
            sparse.save_npz('data_preprocessing/test1_x_sparse_two_'+str(k)+'.npz',test1_x_sparse)
            print('test2_x...')
            sparse.save_npz('data_preprocessing/test2_x_sparse_two_'+str(k)+'.npz',test2_x_sparse)
            print('Over')
            train_part_x_sparse=pd.DataFrame()
            evals_x_sparse=pd.DataFrame()
            test1_x_sparse=pd.DataFrame()
            test2_x_sparse=pd.DataFrame()
```

