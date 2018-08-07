1. [Quick Introduction to Boosting Algorithms in Machine Learning](https://www.analyticsvidhya.com/blog/2015/11/quick-introduction-boosting-algorithms-machine-learning/)
2. [Complete Guide to Parameter Tuning in Gradient Boosting (GBM) in Python](https://www.analyticsvidhya.com/blog/2016/02/complete-guide-parameter-tuning-gradient-boosting-gbm-python/)  [译文地址, 本文主要的参考资料](https://blog.csdn.net/han_xiaoyang/article/details/52663170)

![img](https://www.analyticsvidhya.com/wp-content/uploads/2016/02/tree-infographic.png) 



## GBM中的参数:

树参数

与Boosting参数

其他模型参数



## 调参过程

调参过程参照:  [用sklearn自带的GradientBoostingClassifier示范调参过程](https://www.analyticsvidhya.com/blog/2016/02/complete-guide-parameter-tuning-gradient-boosting-gbm-python/)

需要调节的参数有:

n_estimators, 固定learning_rate后, 使用网格搜索确定

learning_rate, 初始时, 默认0.1, 最优值需使用网格搜索

max_depth, 

max_features, 树节点分裂, 寻找最优分隔特征时, 最多使用多少个特征. 一般先默认设为'sqrt', 具体需网格搜索.

subsample

min_samples_split

min_samples_leaf



### 过程总结起来就是:

1.跑一遍默认参数的模型, 看看效果, 用来作为之后调参的对照.

2.调参顺序,按影响高低排序, 先调整最重要的参数, 就是对模型性能影响最大的参数. 大体顺序: n_estimators, max_depth, min_samples_split, min_samples_leaf, max_features, subsample.

3.大体过程: 先固定其他参数, 找到一个局部最好的n_estimators:

learning_rate先取一个相对比较大的值, 通常0.1, 或者在0.05~0.2之间.

其他参数先设一个默认值, 然后再通过网格搜索确定n_estimators, 一般先搜一个小的范围. 如果最优值在范围边界, 再调整范围.

4.然后调整与树相关的参数, 调整过程与3类似.

5.最后等所有参数都调好, 缩小learning_rate, 等比例放大n_estimators.



## 模型验证,网格搜索调参:

```python
from sklearn.ensemble import GradientBoostingClassifier
from sklearn import cross_validation, metrics
from sklearn.grid_search import GridSearchCV

#交叉验证,输出特征重要性.
def modelfit(alg, X, Y, valid_X, valid_Y, performCV=True, printFeatureInportance=True, cv_folds=5):
    alg.fit(X, Y)
    pred = alg.predict(valid_X)
    predprob = alg.predict_proba(valid_X)[:,1]
    if performCV:
        cv_score = cross_validation.cross_val_score(alg, X, Y, cv=cv_folds)
    print( '\nModel Report')
    print ("Accuracy : %.4f" % metrics.accuracy_score(valid_Y, pred))
    print ("AUC score (Train): %f" % metrics.roc_auc_score(valid_Y, predprob))
    
    if performCV:
        print('CV Score : Mean - %.7g | Std - %.7g | Min - %.7g | Max - %.7g' % (np.mean(cv_score), np.std(cv_score), np.min(cv_score), np.max(cv_score)))
    
    if printFeatureInportance:
        feat_imp = pd.Series(alg.feature_importances_, X.columns).sort_values(ascending=False)
        feat_imp.plot(kind='bar', title='Feature Importances')
        plt.ylabel('Feature Importance Score')
        
gbm0 = GradientBoostingClassifier(random_state=10)
modelfit(gbm0, X_train, Y_train, X_valid, Y_valid)

#网格搜索, 以n_estimators为例:
param_test1 = {'n_estimators':range(20, 81, 10)}
gsearch1 = GridSearchCV(estimator=GradientBoostingClassifier(learning_rate=0.1, min_samples_split=3, 						min_samples_leaf=2,max_depth=5, max_features='sqrt', subsample=0.8, random_state=10),
                 param_grid = param_test1, scoring='accuracy', n_jobs=4, iid=False, cv=5)
gsearch1.fit(X_train, Y_train)
print(gsearch1.grid_scores_, gsearch1.best_params_, gsearch1.best_score_)
```



