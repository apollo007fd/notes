
## Build a Fully Connected Neural Network on MNIST dataset

![](https://camo.githubusercontent.com/269f47b8185a2ca349ead57db511250553fd918b/687474703a2f2f63733233316e2e6769746875622e696f2f6173736574732f6e6e312f6e657572616c5f6e6574322e6a706567)


```python
import tensorflow as tf
import os
```


```python
os.environ['CUDA_VISIBLE_DEVICES'] = "0"
```


```python
from tensorflow.examples.tutorials.mnist import input_data
```


```python
mnist = input_data.read_data_sets('../MNIST_data/', one_hot=True)
```

    Extracting ../MNIST_data/train-images-idx3-ubyte.gz
    Extracting ../MNIST_data/train-labels-idx1-ubyte.gz
    Extracting ../MNIST_data/t10k-images-idx3-ubyte.gz
    Extracting ../MNIST_data/t10k-labels-idx1-ubyte.gz



```python
## 设置网络参数
num_input = 784
n_hidden_1 = 256
n_hidden_2 = 256
n_classes = 10

## 设置其他参数
learning_rate = 0.001
train_steps = 1000
display_steps = 50
batch_size = 128
```


```python
## 定义神经网络权值
weights = {
    'h1': tf.Variable(tf.random_normal([num_input, n_hidden_1])),
    'h2': tf.Variable(tf.random_normal([n_hidden_1, n_hidden_2])),
    'out': tf.Variable(tf.random_normal([n_hidden_2, n_classes]))
}
## 定义每个神经元的biases
biases = {
    'b1': tf.Variable(tf.random_normal([n_hidden_1])),
    'b2': tf.Variable(tf.random_normal([n_hidden_2])),
    'out': tf.Variable(tf.random_normal([n_classes]))
}
## 定义网络输入, 网络输出的占位符
X = tf.placeholder(tf.float32, [None, num_input])
Y = tf.placeholder(tf.float32, [None, n_classes])
```


```python
## 构造函数用来创建神经网络结构,也就是一个计算图
def neural_net(x):
    layer_1 = tf.add(tf.matmul(x, weights['h1']), biases['b1'])
    layer_2 = tf.add(tf.matmul(layer_1, weights['h2']), biases['b2'])
    out_layer = tf.add(tf.matmul(layer_2, weights['out']), biases['out'])
    return out_layer
```


```python
## logits是神经网络的输出
logits = neural_net(X)
```


```python
## 定义损失函数
loss_op = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=logits, labels=Y))
```


```python
## 定义优化器, 定义优化目标
optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate)
train_op = optimizer.minimize(loss_op)
```


```python
## 求测试准确率
correct_pred = tf.equal(tf.argmax(logits, 1), tf.argmax(Y, 1))
accuracy = tf.reduce_mean(tf.cast(correct_pred, dtype=tf.float32))
```


```python
## 定义网络初始化操作
init = tf.global_variables_initializer()
```


```python
## 开始训练网络
with tf.Session() as sess:
    
    sess.run(init)   ## 执行网络初始化

    for i in range(1, train_steps+1):
        batch_x, batch_y = mnist.train.next_batch(batch_size=batch_size)  ## 取一个batch
        sess.run(train_op, feed_dict={X:batch_x, Y:batch_y})  ## 一次训练
        if i % display_steps == 0 or i == 1:
            loss, acc = sess.run([loss_op, accuracy], feed_dict={X:batch_x, Y:batch_y})   ## 计算并打印loss和accuracy
            print("step %d, loss is %.2f, accuracy is %.2f."%(i, loss, acc))
    ## 训练结束, 打印test accuracy
    acc = sess.run(accuracy, feed_dict={X:mnist.test.images, Y:mnist.test.labels})
    print("%.2f"%acc)
```

    step 1, loss is 2628.36, accuracy is 0.09.
    step 50, loss is 427.25, accuracy is 0.68.
    step 100, loss is 343.80, accuracy is 0.70.
    step 150, loss is 173.75, accuracy is 0.85.
    step 200, loss is 89.12, accuracy is 0.85.
    step 250, loss is 168.75, accuracy is 0.84.
    step 300, loss is 173.33, accuracy is 0.80.
    step 350, loss is 101.27, accuracy is 0.86.
    step 400, loss is 107.13, accuracy is 0.85.
    step 450, loss is 162.43, accuracy is 0.81.
    step 500, loss is 128.48, accuracy is 0.84.
    step 550, loss is 99.43, accuracy is 0.83.
    step 600, loss is 113.21, accuracy is 0.85.
    step 650, loss is 130.72, accuracy is 0.84.
    step 700, loss is 87.58, accuracy is 0.88.
    step 750, loss is 40.89, accuracy is 0.94.
    step 800, loss is 95.45, accuracy is 0.88.
    step 850, loss is 99.92, accuracy is 0.82.
    step 900, loss is 87.03, accuracy is 0.89.
    step 950, loss is 114.79, accuracy is 0.80.
    step 1000, loss is 29.24, accuracy is 0.95.
    0.87


## 2层全连接神经网络总结

### 设置参数:

关于网络结构的参数:

- n_layer_1, n_layer_2 (每一层神经元的个数)
- num_inputs (输入特征个数)
- n_classes (输出参数个数)

其他参数:

- learning_rate
- batch_size
- train_steps  (训练多少次)
- display_steps (多少次输出一次训练结果: loss/accuracy)

### 定义神经网络weights和biases:

weights和biases分别定义成一个字典, 2层全连接神经网络, 需要定义3个weight矩阵, 3个biases矩阵

tf.Variable(tf.random_normal(shape))

### 定义神经网络输入/输出占位符:

tf.placeholder(dtype, shape)

### 构造神经网络:

定义前向传播过程, 用到tf.add(), tf.matmul()

### 定义求损失\准确率的计算:

loss_op = tf.reduce_mean(tf.softmax_cross_entropy_with_logits(logits=logits, labels=Y))

correct_pred = tf.equal(tf.argmax(logits, 1), tf.argmax(Y, 1))

accuracy = tf.reduce_mean(tf.cast(correct_pred, tf.float32))

### 定义网络优化器:

tf.train.xxx(learning_rate).minimize(loss_op)

### 定义初始化器:

init = tf.global_variable_initializer()

### 开始训练网络:

初始化参数: sess.run(init)

取一个batch: batch_x, batch_y=mnist.train.next_batch(batch_size=batch_size)
