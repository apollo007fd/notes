# GDBT原理与实践

简单地来说，提升就是指每一步我都产生一个弱预测模型，然后加权累加到总模型中，然后每一步弱预测模型生成的的依据都是损失函数的负梯度方向，这样若干步以后就可以达到逼近损失函数局部最小值的目标。 



------



## [知乎：机器学习算法中 GBDT 和 XGBOOST 的区别有哪些？](https://www.zhihu.com/question/41354392)

[wepon](https://www.zhihu.com/people/wepon-huang) 的回答



### [GBDT算法原理与系统设计简介 ](http://wepon.me/files/gbdt.pdf)

大纲:

• 泰勒公式 

• 最优化方法

 • 梯度下降法（Gradient descend method）

 • 牛顿法（Newton’s method)

 • 从参数空间到函数空间 

• 从Gradient descend 到 Gradient boosting 

• 从Newton’s method 到 Newton Boosting 

• Gradient Boosting Tree 算法原理 

• Newton Boosting Tree 算法原理：详解 XGBoost 

• 更高效的工具包 LightGBM 

• 参考文献 



#### 泰勒公式

**定义**：泰勒公式是一个用函数在某点的信息描述其**附近**取值的公式。(**局部有效性**)

**基本形式**: $f(x)=\sum_{n=0}^{\infty} {\frac{f^{(n)}(x_0)}{n!}(x-x_0)^n}$

- 一阶泰勒展开: $f(x) \simeq f(x_0) + f^{'}(x_0)(x-x_0)$
- 二阶泰勒展开: $f(x) \simeq f(x_0) +  f^{'}(x_0)(x-x_0) +  f^{''}(x_0) \frac {(x-x_0)^2} {2}$

**迭代形式**: 假设$x^t = x^{t-1} + \Delta x$, 将$f(x^t)在$$x^{t-1}$处进行泰勒展开:

$f(x^t) = f(x^{t-1} + \Delta x) = f(x^{t-1}) + f^{'}(x^{t-1})\Delta x + f^{''}(x^{t-1}) \frac {\Delta x^2} {2} $



#### 梯度下降法

在机器学习任务中,需要最小化损失函数$L(\theta)$, 其中$\theta$ 是要求解的模型参数. 梯度下降法常用来求解这种无约束最优化问题,它是一种迭代方法: 选取初值$\theta ^0$, 不断迭代, 更新$\theta$ 值, 进行损失函数的极小化.

- 迭代公式: $\theta ^ t = \theta^{t-1} + \Delta\theta$
- 将$L(\theta^t)$ 在$\theta^{t-1}$处进行一阶泰勒展开:

$$
L(\theta^t) = L(\theta ^{t-1}+\Delta\theta)
\simeq L(\theta^{t-1}) + L^{'}(\theta^{t-1})\Delta\theta
$$

-  要使得$L(\theta^t) < L(\theta^{t-t}) $ , 可取: $\Delta \theta = -\alpha L^{'}(\theta ^{t-1})$ , 则: $\theta^t = \theta^{t-1} - \alpha L^{'}(\theta^{t-1})$ 



#### 牛顿法 (Newton's Method)

- 将$L(\theta^t)$ 在$\theta^{t-1}$处进行二阶泰勒展开:

  $L(\theta^t) = L(\theta ^{t-1}+\Delta\theta)$
  $\simeq L(\theta^{t-1}) + L^{'}(\theta^{t-1})\Delta\theta + L^{''}(\theta^{t-1})\frac {\Delta \theta ^ 2} {2}$	

为了简化分析过程, 假设参数是标量(即$\theta$只有一维), 则可以将一阶和二阶导数分别记为g和h: 

$L(\theta ^ t) = L(\theta ^{t-1}) + g \Delta \theta + h \frac {\Delta \theta ^ 2} {2}$

- 要使得$L(\theta^t)$极小, 即让$ g \Delta \theta + h \frac {\Delta \theta ^ 2} {2}$ 极小, 可令: $\frac { \partial  ({g \Delta \theta + h \frac {\Delta \theta ^ 2} {2})}} {\partial  \Delta \theta}$ = 0

  求得$\Delta \theta = - \frac g h$ , 故 $\theta ^ t = \theta ^{t-1} + \Delta \theta = \theta^{t-1} - \frac g h$

  参数$\theta$推广到向量形式,迭代公式: $\theta ^ t = \theta ^ {t-1} -H^{-1}g$

  这里H是海森矩阵



#### 从参数空间到函数空间

- GDBT 在函数空间中利用梯度下降法进行优化

- XGBoost 在函数空间中利用牛顿法进行优化

  

------

