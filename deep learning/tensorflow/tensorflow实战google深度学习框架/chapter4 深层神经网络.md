
#### 非线性激活函数
tf.nn.relu, tf.nn.sigmoid, tf.nn.tanh

#### 损失函数


```python
# 交叉熵: 刻画2个概率分布之间的距离
# cross_entropy = -tf.reduce_mean( y_ * tf.log(tf.clip_by_value(y, 1e-10, 1.0)))
```


```python
import tensorflow as tf
v = tf.constant([[1.0, 2.0, 3.0],[4.0, 5.0, 6.0]])
```


```python
v
```


```python
sess = tf.Session()
```


```python
# tf.clip_by_value(v, low_bound, high_bound)
# 不指定session将报错, ValueError: Cannot evaluate tensor using `eval()`: No default session is registered. Use `with sess.as_default()` or pass an explicit session to `eval(session=sess)
print(tf.clip_by_value(v, 2.5, 4.5).eval(session=sess))
```


```python
v = tf.constant([1.0, 2.0, 3.0])
```


```python
# tf.log(v)
print(tf.log(v).eval(session=sess))
```


```python
# 矩阵点乘 "*"
v1 = tf.constant([[1.0, 2.0],[3.0, 4.0]])
v2 = tf.constant([[5.0, 6.0],[7.0, 8.0]])
print((v1 * v2).eval(session=sess))
```


```python
# 矩阵乘积 tf.matmul
print(tf.matmul(v1, v2).eval(session=sess))
```


```python
# 矩阵平均值 tf.reduce_mean(v)
v = tf.constant([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
print(tf.reduce_mean(v).eval(session=sess))
```


```python
# tensorflow 将 softmax回归 和 计算交叉熵 合并:
# tf.nn.softmax_cross_entropy_with_logits(y, y_)
# y是神经网络的输出, y_代表标准答案
```


```python
# 对于只有一个正确答案的分类问题, tensorflow提供了如下接口:
# tf.nn.sparse_softmax_cross_entropy_with_logits()
```


```python
# 均方误差 MSE, y_代表标准答案, y代表神经网络的输出
# tf.reduce_mean(tf.square(y_-y))
```

自定义损失函数:


```python
# 以商品利润预测为例,损失函数:
# loss = tf.reduce_sum(tf.select( tf.greater(v1, v2), (v1-v2)*a, (v1-v2)*b ))
# tf.select(condition, case1, case2)
# 1.2中已经没有tf.select(), 只有tf.where(), 功能是一样的.
```


```python
sess.close()
```


```python
import tensorflow as tf
v1 = tf.constant([1.0, 2.0, 3.0, 4.0])
v2 = tf.constant([4.0, 3.0, 2.0, 1.0])
sess = tf.InteractiveSession()
print(tf.greater(v1, v2).eval())
```


```python
## module 'tensorflow' has no attribute 'select'
#print(tf.select(tf.greater(v1, v2).eval(), v1, v2).eval())
```


```python
print(tf.where(tf.greater(v1, v2).eval(), v1, v2).eval())
```

损失函数对模型训练结果的影响:


```python
import tensorflow as tf
from numpy.random import RandomState
```


```python
batch_size = 8

x = tf.placeholder(dtype=tf.float32, shape=(None, 2), name="x-input")
y_ = tf.placeholder(dtype=tf.float32, shape=(None, 1), name="y-input")
```


```python
w1 = tf.Variable(tf.random_normal([2,1], stddev=1, seed=1))
y = tf.matmul(x, w1)
```


```python
# 自定义损失函数loss
# 选择优化方法,学习率,优化目标(损失函数)
# tf.reduce_sum() tf.where tf.greater()
# tf.train.AdamOptimizer(lr).minimize(loss)
loss_less = 1
loss_more = 10
loss = tf.reduce_sum(tf.where(tf.greater(y, y_), loss_more*(y-y_), loss_less*(y_-y)))
train_step = tf.train.AdamOptimizer(0.001).minimize(loss)
```


```python
rdm = RandomState(1)
```


```python
dataset_size = 128
X = rdm.rand(dataset_size, 2)   ## numpy.random.RandomState 产生[0,1)的均匀分布
```


```python
# 设置Y = x1 + x2 + small noise
Y = [[x1+x2+rdm.rand()/10-0.05] for x1,x2 in X]  ## noise: rdm.rand()/10 -0.05 是[-0.05, 0.05)的均匀分布
```


```python
with tf.Session() as sess:
    init_op = tf.global_variables_initializer()
    sess.run(init_op)
    STEPS = 5000
    for i in range(STEPS):
        start = (i * batch_size) % dataset_size
        end = min(start+batch_size, dataset_size)
        sess.run(train_step, feed_dict={x:X[start:end], y_:Y[start:end]})
        print(sess.run(w1))
```

总结:  
当loss_more=1, loss_less=10时,w1=[1.019, 1.043], 倾向于预测大一些;  
当loss_more=10, loss_less=1时,w1=[0.974, 1.024], 倾向于预测小一些;

#### 神经网络优化算法

学习率的设置--指数衰减法


```python
#decayed_learning_rate = learning_rate * decay_rate ^ (global_step / decay_steps)
```

#### 过拟合问题

正则化


```python
## 简单的带L2正则化的损失函数定义:
#w = tf.Variable(tf.random_normal([2,1], stddev=1, seed=1))
#y = tf.matmul(x, w)
#loss = tf.reduce_mean(tf.square(y_-y)) + tf.contrib.layersers.l2_regularizer(lambda)(w)
```


```python
weights = tf.constant([[1.0, 2.0], [3.0, 4.0]])
with tf.Session() as sess:
    # (1+2+3+4)*0.5=5
    print(sess.run(tf.contrib.layers.l1_regularizer(.5)(weights)))
    # (1+2*2+3*3+4*4)/2*0.5=7.5
    print(sess.run(tf.contrib.layers.l2_regularizer(0.5)(weights)))
```

    5.0
    7.5


当网络结构复杂时,可以通过集合计算一个5层神经网络带L2正则化损失函数的计算方法:


```python
import tensorflow as tf
```


```python
# 获取一层神经网络边上的权重,并将这个权重的L2正则化损失加入名称为'losses'的集合中
def get_weight(shape, lamda):
    # 生成一个变量
    var = tf.Variable(tf.random_normal(shape), dtype=tf.float32)
    # tf.add_to_collection函数将这个新生成变量的L2正则化损失项加入集合
    tf.add_to_collection('losses', tf.contrib.layers.l2_regularizer(lamda)(var))
    # 返回生成的变量
    return var
```


```python
x = tf.placeholder(tf.float32, shape=(None, 2))
y_ = tf.placeholder(tf.float32, shape=(None,1))
batch_size=8
```


```python
# 定义了每一层网络中节点的个数
layer_dimension = [2,10,10,10,1]
```


```python
# 神经网络的层数
n_layers = len(layer_dimension)
```


```python
# 这个变量维护前向传播时最深层的节点,开始的时候就算输入层
cur_layer = x
```


```python
# 当前层的节点个数
in_dimension= layer_dimension[0]
```


```python
# 通过一个循环来生成5层全连接层的神经网络结构
for i in range(1, n_layers):
    # layer_dimension[i] 为下一层的节点个数
    out_dimension = layer_dimension[i]
    # 生成当前层中权重的变量,并将这个变量的L2正则化损失加入计算图上的集合
    weight = get_weight([in_dimension, out_dimension], 0.001)
    bias = tf.Variable(tf.constant(0.1, shape=[out_dimension]))   # shape传入的参数就算是1维的,也要是一个list,不能为单个数
    # 使用relu激活函数,得到当前层的输出
    cur_layer = tf.nn.relu(tf.matmul(cur_layer, weight)+ bias)
    # 进入下一层之前,将下一层的输入节点个数更新为当前层的节点个数
    in_dimension = out_dimension
```


```python
# 在定义神经网络前向传播的同时依据将所有的L2正则化损失加入了图上的集合
# 这里只需要计算刻画模型在训练数据上表现的损失函数
mse_loss = tf.reduce_mean(tf.square(y_ - cur_layer))
```


```python
# 将均方误差损失函数加入损失集合
tf.add_to_collection('losses', mse_loss)
```


```python
# get_collection返回一个列表,这个列表是所有这个集合的元素.在这个样例中,
# 这些元素就是损失函数的不同部分,将它们加起来就可以得到最终的损失函数.
loss = tf.add_n(tf.get_collection('losses'))
```

#### 滑动平均模型

略
