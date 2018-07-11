

```python
import tensorflow as tf
```

# 计算图的使用


```python
a = tf.constant([1.0, 2.0], name="a")
b = tf.constant([2.0, 3.0], name="b")
```


```python
result = a + b
```


```python
print(a.graph is tf.get_default_graph())
```

    True



```python
# tensorflow支持通过tf.Graph函数来生成新的计算图
g1 = tf.Graph()
with g1.as_default():
    # 在计算图g1中定义变量v，并设置初始值为0
    v = tf.get_variable("v",initializer=tf.zeros_initializer()(shape = [1]))

g2 = tf.Graph()
with g2.as_default():
    # 在计算图g2中定义变量v，并设置初始值为1
    v = tf.get_variable( "v",initializer=tf.ones_initializer()(shape = [1]))

# 在计算图g1中读取变量v的取值
with tf.Session(graph=g1) as sess:
    tf.global_variables_initializer().run()
    with tf.variable_scope("", reuse=True):
        # 在计算图g1中，变量v的取值应该为0，所以下面的输出为[0.]
        print(sess.run(tf.get_variable("v")))

# 在计算图g2中读取变量v的取值
with tf.Session(graph=g2) as sess:
    tf.global_variables_initializer().run()
    with tf.variable_scope("", reuse=True):
        # 计算图g2中，变量v的取值应该为1，所以下面这行会输出[1.]
        print(sess.run(tf.get_variable("v")))
```

    [0.]
    [1.]


# 张量


```python
a = tf.constant([1, 2], name="a", dtype=tf.float32)   # 不加dtype=tf.float32, a+b相加会报类型不匹配错误
b = tf.constant([2.0, 3.0], name="b")
result = a + b
```

# 会话


```python
# 创建一个会话
sess = tf.Session()
print(sess.run(result))
sess.close()
```

    [3. 5.]



```python
# 为避免会话异常结束,没有正常关闭; 使用with语法创建会话
with tf.Session() as sess:
    print(sess.run(a+b))
```

    [3. 5.]



```python
# 通过设定默认会话计算张量的值:
sess = tf.Session()
with sess.as_default():
    print(result.eval())
```

    [3. 5.]


# Tensorflow实现神经网络


```python
# tf.Variable, tensorflow变量的声明函数
# tf.random_normal可以指定均值mean, 不指定时默认为0; 可以指定方差stddev
weights = tf.Variable(tf.random_normal([2,3], stddev=2))
```


```python
weights
```




    <tf.Variable 'Variable:0' shape=(2, 3) dtype=float32_ref>




```python
# 生成一个初始值全为0,长度为3的变量
biases = tf.Variable(tf.zeros([3]))
```


```python
biases
```




    <tf.Variable 'Variable_1:0' shape=(3,) dtype=float32_ref>




```python
c = tf.constant([2,3,4])
```


```python
c
```




    <tf.Tensor 'Const:0' shape=(3,) dtype=int32>




```python
# w2初始值设置成与weights变量相同
w2 = tf.Variable(weights.initialized_value())
```


```python
w2
```




    <tf.Variable 'Variable_2:0' shape=(2, 3) dtype=float32_ref>




```python
w3 = tf.Variable(weights.initialized_value() * 2.0)
```


```python
w3
```




    <tf.Variable 'Variable_3:0' shape=(2, 3) dtype=float32_ref>



#### 神经网络的前向传递:


```python
import tensorflow as tf

w1 = tf.Variable(tf.random_normal([2,3], stddev=1, seed=1))
w2 = tf.Variable(tf.random_normal([3,1], stddev=1, seed=1))
```


```python
# shape = (1,2)
x = tf.constant([[0.7, 0.9]])
```


```python
# shape = (1,3)
a = tf.matmul(x, w1)
```


```python
# shape = (1,1)
y = tf.matmul(a, w2)
```


```python
sess = tf.Session()
```


```python
## 分2步初始化w1, w2, 也可以一次性初始化全部变量;
sess.run(w1.initializer)
sess.run(w2.initializer)
## 一次性初始化全部变量:
#init_op = tf.initialize_all_variables()
#sess.run(init_op)
```


```python
# 执行计算y
print(sess.run(y))
```

    [[3.957578]]



```python
sess.close()
```

## 张量


```python
# 变量声明函数tf.Variable是一个运算,这个运算的输出是一个张量
```


```python
# GraphKeys.VARIABLES   -->所有变量都会自动的加入这个集合
# GraphKeys.TRAINABLE_VARIABLES  -> tensorflow会将此集合中的变量作为默认的优化对象
# tensorflow中可以用tf.trainable_variables函数得到所有需要优化的参数
```


```python
# 类似tensor, 一个变量的2个重要参数,shape, dtype
# 一个变量在构建以后,它的类型不可改变.
# 举个反例:
```


```python
## 下面代码将报错,w1的type没有指定,默认为tf.float32, 那么将不能被赋予其他类型的值
# w1 = tf.Variable(tf.random_normal([2,3], stddev=1), name='w1')
# w2 = tf.Variable(tf.random_normal([2,3], dtype=tf.float64, stddev=1), name='w2')
# w1.assign(w2)
```


```python
# 维度在程序的运行过程中是有可能改变的,但是需要通过设置参数validate_shape=False
```


```python
w1 = tf.Variable(tf.random_normal([2,3], stddev=1), name='w1')
w2 = tf.Variable(tf.random_normal([2,3], stddev=1), name='w2')
```


```python
# w1\w2维度相同,赋值将会成功
tf.assign(w1, w2)
```




    <tf.Tensor 'Assign_1:0' shape=(2, 3) dtype=float32_ref>




```python
w1 = tf.Variable(tf.random_normal([2,3], stddev=1), name='w1')
w2 = tf.Variable(tf.random_normal([2,2], stddev=1), name='w2')
```


```python
## 下面这句将报错:ValueError: Dimension 1 in both shapes must be equal, but are 3 and 2 for 'Assign_2' (op: 'Assign') with input shapes: [2,3], [2,2].
#tf.assign(w1, w2)
```


```python
# 设置了validate_shape=False,不会报错
tf.assign(w1, w2, validate_shape=False)
```




    <tf.Tensor 'Assign_3:0' shape=(2, 2) dtype=float32_ref>



# 通过tensorflow训练神经网络模型


```python
# batch的表示-->placeholder, 不使用tf.constant来表示batch, 是因为每个constant会在计算图中生成一个节点,如果迭代次数太高,会导致计算图太大;
# placeholder的类型需要指定,且是不可以改变的
# placeholder的维度不需要指定,会根据提供的数据推导得出
```

#### 通过placeholder实现前向传播算法:


```python
import tensorflow as tf

w1 = tf.Variable(tf.random_normal([2,3], stddev=1))
w2 = tf.Variable(tf.random_normal([3,1], stddev=1))

x = tf.placeholder(tf.float32, shape=(1,2), name="input")
a = tf.matmul(x, w1)
y = tf.matmul(a, w2)

sess = tf.Session()
init_op = tf.global_variables_initializer()
sess.run(init_op)
```


```python
## 报错: InvalidArgumentError: You must feed a value for placeholder tensor 'input' with dtype float and shape [1,2]
#print(sess.run(y))
```


```python
print(sess.run(y, feed_dict={x: [[0.7, 0.9]]}))
```

    [[1.9052694]]


上面的代码只计算了一个样例的前向传播结果,但是有时候每次需要提供一个batch的训练样例.
对于这样的需求,placeholder也可以很好的支持.在上面的样例程序中,如果将输入的1x2矩阵改为nx2的矩阵,那么就可以得到n个样例的前向传播结果了.
其中nx2的矩阵的每一行为一个样例数据.这样前向传播的结果为nx1的矩阵.


```python
import tensorflow as tf

w1 = tf.Variable(tf.random_normal([2,3], stddev=1))
w2 = tf.Variable(tf.random_normal([3,1], stddev=1))

# shape[0]=3, batchsize=3
x = tf.placeholder(tf.float32, shape=(3,2), name="input")  
a = tf.matmul(x, w1)
y = tf.matmul(a, w2)

sess = tf.Session()
init_op = tf.global_variables_initializer()
sess.run(init_op)
```


```python
# 输出 shape = (bathsize, 1)
print(sess.run(y, feed_dict={x: [[0.7, 0.9],[0.1, 0.4], [0.5, 0.8]]}))
```

    [[3.4851317 ]
     [0.91387004]
     [2.7302184 ]]


在得到前向传播结果后,需要定义一损失函数,来刻画当前的预测值和真实值之间的差距.然后通过反向传播算法来调整神经网络的参数, 使得差距可以被缩小.(见chapter4)


```python
#cross_entropy = -tf.reduce_mean(
#    y_ * tf.log(tf.clip_by_value(y, 1e-10, 1)))
#learning_rate = 0.01
#train_step = tf.train.AdamOptimizer(learning_rate).minimize(cross_entropy)
```

# 神经网络解决二分类问题


```python
import tensorflow as tf
from numpy.random import RandomState
```


```python
# 设置batch size
batch_size = 8
```


```python
# 设置权重
w1 = tf.Variable(tf.random_normal([2,3], stddev=1, seed=1))
w2 = tf.Variable(tf.random_normal([3,1], stddev=1, seed=1))
```


```python
# 设置输入输出占位符
x = tf.placeholder(dtype=tf.float32, shape=(None, 2), name="x-input")
y_ = tf.placeholder(dtype=tf.float32, shape=(None, 1), name='y-input')
```


```python
# 定义神经网络前向传播
a = tf.matmul(x, w1)
y = tf.matmul(a, w2)
```


```python
# 定义损失函数, 优化步骤
cross_entropy = -tf.reduce_mean(
        y_ * tf.log(tf.clip_by_value(y, 1e-10, 1.0)))
train_step = tf.train.AdamOptimizer(0.001).minimize(cross_entropy)
```


```python
rdm = RandomState(1)
```


```python
dataset_size= 128
```


```python
X = rdm.rand(dataset_size, 2)
```


```python
Y = [[int(x1+x2 < 1)] for (x1, x2) in X]
```


```python
with tf.Session() as sess:
    init_op = tf.global_variables_initializer()
    sess.run(init_op)
    print(sess.run(w1))
    print(sess.run(w2))
    
    STEPS = 5000
    for i in range(STEPS):
        start = (i * batch_size) % dataset_size
        end = min(start+batch_size, dataset_size)
        
        sess.run(train_step, feed_dict={x:X[start:end], y_:Y[start:end]})
        if i % 1000 == 0:
            total_cross_entropy = sess.run(cross_entropy, feed_dict={x: X, y_:Y})
            print('After %d training step(s), cross entropy on all data is %g'%(i, total_cross_entropy))
    
    print(sess.run(w1))
    print(sess.run(w2))
```

    [[-0.8113182   1.4845988   0.06532937]
     [-2.4427042   0.0992484   0.5912243 ]]
    [[-0.8113182 ]
     [ 1.4845988 ]
     [ 0.06532937]]
    After 0 training step(s), cross entropy on all data is 0.0674925
    After 1000 training step(s), cross entropy on all data is 0.0163385
    After 2000 training step(s), cross entropy on all data is 0.00907547
    After 3000 training step(s), cross entropy on all data is 0.00714436
    After 4000 training step(s), cross entropy on all data is 0.00578471
    [[-1.9618274  2.582354   1.6820378]
     [-3.4681718  1.0698233  2.1178901]]
    [[-1.824715 ]
     [ 2.6854665]
     [ 1.4181951]]

