
Build a convolutional neural network on MNIST dataset
![image.png](https://camo.githubusercontent.com/afc8cecd033ab300799ceb2bf3b593efa3bda2b7/687474703a2f2f706572736f6e616c2e69652e6375686b2e6564752e686b2f7e63636c6f792f70726f6a6563745f7461726765745f636f64652f696d616765732f666967332e706e67)


```python
import tensorflow as tf
import os
```


```python
from tensorflow.examples.tutorials.mnist import input_data
```


```python
os.environ['CUDA_VISIBLE_DEVICES'] = "0"
```


```python
mnist = input_data.read_data_sets('../MNIST_data/', one_hot=True)
```

    Extracting ../MNIST_data/train-images-idx3-ubyte.gz
    Extracting ../MNIST_data/train-labels-idx1-ubyte.gz
    Extracting ../MNIST_data/t10k-images-idx3-ubyte.gz
    Extracting ../MNIST_data/t10k-labels-idx1-ubyte.gz



```python
## 定义训练参数
learning_rate = 0.001
num_steps = 1000
display_step = 100
batch_size = 128
```


```python
## 定义网络参数
num_input = 784
num_classes = 10
droupout = 0.75
```


```python
## 定义网络的输入\输出占位符
X = tf.placeholder(tf.float32, [None, 784])
Y = tf.placeholder(tf.float32, [None, 10])
keep_prob = tf.placeholder(tf.float32)
```


```python
## 函数: 用来构造一层卷积层, W是filter
def conv2d(x, W, b, strides=1):
    x = tf.nn.conv2d(x, W, strides=[1, strides, strides, 1], padding='SAME')
    x = tf.nn.bias_add(x, b)
    return tf.nn.relu(x)
```


```python
## 函数: 用来构造一层池化层
def maxpool2d(x, k=2):
    return tf.nn.max_pool(x, ksize=[1, k, k, 1], strides=[1, k, k, 1], padding="SAME")
```


```python
## 函数: 用来构造卷积神经网络
def conv_net(x, weights, biases, dropout):
    
    x = tf.reshape(x, shape=[-1, 28, 28, 1])
    
    conv1 = conv2d(x, weights['wc1'], biases['bc1'])
    conv1 = maxpool2d(conv1, k=2)
    
    conv2 = conv2d(conv1, weights['wc2'], biases['bc2'])
    conv2 = maxpool2d(conv2, k=2)
    
    fc1 = tf.reshape(conv2, shape=[-1, weights['wd1'].get_shape().as_list()[0]])
    fc1 = tf.add(tf.matmul(fc1, weights['wd1']), biases['bd1'])
    fc1 = tf.nn.relu(fc1)
    
    fc1 = tf.nn.dropout(fc1, keep_prob=droupout)
    out = tf.add(tf.matmul(fc1, weights['out']), biases['out'])
    
    return out
```


```python
# 第1层卷积后shape:[batch_size,28,28,32], 第1层池化后shape:[batch_size,14,14,32]
# 第2层卷积后shape:[batch_size,14,14,64], 第2层池化后shape:[batch_size,7,7,64]
# 第1层全连接后shape:[batch_size,1024] 
# 第2层全连接后shape:[batch_size, 10] 
weights = {
    'wc1': tf.Variable(tf.random_normal(dtype=tf.float32, shape=[5, 5, 1, 32])),   
    'wc2': tf.Variable(tf.random_normal(dtype=tf.float32, shape=[5, 5, 32, 64])),  
    'wd1': tf.Variable(tf.random_normal(dtype=tf.float32, shape=[7*7*64, 1024])), 
    'out': tf.Variable(tf.random_normal(dtype=tf.float32, shape=[1024, num_classes]))      
}
```


```python
biases = {
    'bc1': tf.Variable(tf.random_normal([32])),  # 第1层卷积层的biases, 个数和第1个卷积核的输出通道数相同
    'bc2': tf.Variable(tf.random_normal([64])),  # 
    'bd1': tf.Variable(tf.random_normal([1024])),
    'out': tf.Variable(tf.random_normal([num_classes]))
}
```


```python
## 构造卷积神经网络
logits = conv_net(X, weights, biases, droupout)
```


```python
## 构造损失函数
loss_op = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=logits, labels=Y))
```


```python
## 构造网络输出
prediction = tf.nn.softmax(logits)
```


```python
## 构造优化器
optimizer = tf.train.AdamOptimizer(learning_rate)
train_op = optimizer.minimize(loss_op)
```


```python
## 构造模型的测试准确率计算操作
correct_pred = tf.equal(tf.argmax(prediction, 1), tf.arg_max(tf.cast(Y, tf.float32), 1))
accuracy = tf.reduce_mean(tf.cast(correct_pred, tf.float32))
```


```python
## 构造模型参数的初始化操作
init = tf.global_variables_initializer()
```


```python
## 训练模型
with tf.Session() as sess:
    sess.run(init)
    for i in range(1, 1+num_steps):
        batch_x, batch_y = mnist.train.next_batch(batch_size)
        sess.run(train_op, feed_dict={X:batch_x, Y:batch_y})
        if i % display_step==0 or i==1:
            loss, acc = sess.run([loss_op, accuracy], feed_dict={X:batch_x, Y:batch_y})
            print('step %d, loss: %.3f, accuracyL %.3f.'%(i, loss, acc))
    # 训练结束,输出测试结果
    acc = sess.run(accuracy, feed_dict={X:mnist.test.images[:1000], Y:mnist.test.labels[:1000]})
    print('accuracy on testset is %.3f.'%acc)
```

    step 1, loss: 109108.438, accuracyL 0.141.
    step 100, loss: 5023.237, accuracyL 0.766.
    step 200, loss: 2764.799, accuracyL 0.859.
    step 300, loss: 638.816, accuracyL 0.953.
    step 400, loss: 1274.484, accuracyL 0.906.
    step 500, loss: 776.960, accuracyL 0.906.
    step 600, loss: 356.664, accuracyL 0.930.
    step 700, loss: 1032.738, accuracyL 0.922.
    step 800, loss: 909.456, accuracyL 0.914.
    step 900, loss: 873.582, accuracyL 0.938.
    step 1000, loss: 406.336, accuracyL 0.938.
    accuracy on testset is 0.941.


构造过程总结:

1. 定义训练参数: learning_rate\num_steps\display_steps\dropout
2. 定义网络参数: num_input\ num_classes
3. 构造输入输出占位符
4. 构造网络结构
5. 定义网络weights\biases 字典
6. 构造计算loss, accuracy的操作 
7. 构造优化器 train_op = tf.train.Adam....(learning_rate).minimize(loss_op)
8. 构造初始化操作 init=tf.global....
9. 训练网络

