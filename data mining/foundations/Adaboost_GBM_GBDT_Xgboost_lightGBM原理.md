[TOC]

# Adaboost

只使用一种基学习算法, 通过改变误分类样本的权重, 串行训练n个弱分类器(也可以用到回归问题). 第一轮训练, 各个样本权重相同. 之后, 每一轮都增加上一轮误分类点的权重. 最后, 一种简单的做法, 对于分类问题, 可以将n个弱分类器的结果进行投票. 对于回归问题, 可以取平均.



参考:

[Quick Introduction to Boosting Algorithms in Machine Learning](https://www.analyticsvidhya.com/blog/2015/11/quick-introduction-boosting-algorithms-machine-learning/)  介绍Adaboost思想, 附有sklearn Adaboost代码





# GBM

调参实例参考: notes\data mining\foundations\GBM算法-网格搜索调参实例.md



参考:

[Complete Guide to Parameter Tuning in Gradient Boosting (GBM) in Python](https://www.analyticsvidhya.com/blog/2016/02/complete-guide-parameter-tuning-gradient-boosting-gbm-python/) 介绍GBM思想, 附GridSearchCV调参代码. [译文](https://blog.csdn.net/han_xiaoyang/article/details/52663170)







# Xgboost

XGBoost是Gradient Boosting(GBM)算法的一个优化的版本.

## Xgboost相比GBM的优势:

正则化提升, [并行处理](http://zhanpengfang.github.io/418home.html) , 高度的灵活性, 缺失值处理, 剪枝, 内置交叉验证, 在已有的模型基础上继续.

## XGBoost的参数:

通用参数, Booster参数, 学习目标参数.

### 1.通用参数:

- ##### booster[默认gbtree]: gbtree/gtliner

- ##### silent[默认0]: 1/0

- ##### nthread[默认值为最大可能的线程数].

### 2.Booster参数 

tree booster/linear booster, 这里只介绍tree booster

- eta[默认0.3]
  - 和GBM中的learning_rate参数类似
  - 通过减少每一步的权重, 可以提高模型的鲁棒性
  - 典型值0.01~0.2

- min_child_weight
  - 默认1
  - 原文说最小叶子节点样本权重和, 没太懂.
  - 需要使用CV来确定
- max_depth
  - 默认6
  - 需CV来确定
  - 典型值3-10
- max_leaf_nodes
  - 树上最大的节点或叶子的数量
  - 如果定义了这个参数, 会忽略掉max_depth
- gamma
  - 在节点分裂时, 只有分裂后损失函数的值下降了, 才会分裂这个节点. Gamma指定了节点分类所需的最小损失函数下降值.
  - 默认0
  - 设置越大, 模型越保守, 需要调整.
- max_delta_step
  - 默认0
  - 参见原文
- subsample
  - 默认1
  - 和GBM中的subsample参数一模一样. 这个参数控制对于每棵树, 随机采样的比例.
  - 减小这个参数的值, 算法会更加保守, 避免过拟合. 但是, 如果这个值设置得过小, 它可能会导致欠拟合.
  - 典型值0.5~1
- colsample_bytree
  - 和GBM里面max_features参数类似, 用来控制每棵树所用的训练集, 随机采样的列数占比.
  - 典型值0.5~1
- colsample_bylevel
  - 默认1
  - 用来控制树的每一级分裂, 对列数的采样的占比.
- lambda
  - 默认1
  - 权重的L2正则化项
- alpha
  - 默认1
  - 权重的L1正则化项
- scale_pos_weight
  - 在各类别样本十分不平衡时, 把这个参数设定为一个正值, 可以使算法更快收敛.

### 3.学习目标参数

这个参数用来控制理想的优化目标和每一步结果的度量方法.

- objective
  - 这个参数定义需要被最小化的损失函数. 最常用的值有:
    - 默认reg:linear
    - binary:logistic 二分类的逻辑回归, 返回预测的概率(不是类别).
    - multi:softmax 使用softmax的多分类器, 返回预测的类别(不是概率).
      - 在这个情况下, 还需要多设置一个参数: num_class(类别参数)
    - multi:softprob 和multi:softmax参数一样, 返回的是每个数据属于各个类别的概率.
- eval_metric
  - 默认值取决于objective参数的取值
  - 对于有效数据的度量方法
  - 对回归问题, 默认值是rmse, 对于分类问题, 默认值是error.
  - 典型值有
    - rmse   	均方根误差 [对 残差平方 求和 求均值 开方]
    - mae        平均绝对误差 [用于分别求定位x,y误差很合适]
    - logloss    负对数似然函数值 [对概率值 取对数 取反 加权]
    - error       二分类错误率 [阈值为0.5]
    - merror    多分类错误率
    - mlogloss 多分类logloss损失函数
    - auc           roc曲线下面积
- seed
  - 随机数种子
  - 设置它可以复现随机数据的结果, 也可以用于调整参数.



XGBoost没有和GBM中n_estimators类似的参数.





参考:

[XGBoost参数调优完全指南](https://blog.csdn.net/han_xiaoyang/article/details/52665396)  CSDN博客 [英文原文](https://www.analyticsvidhya.com/blog/2016/03/complete-guide-parameter-tuning-xgboost-with-codes-python/)

XGBoost并行计算 [参考网页](http://zhanpengfang.github.io/418home.html)