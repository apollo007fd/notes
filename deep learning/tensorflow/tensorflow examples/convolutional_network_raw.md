
$y=k^x + b$
![image.png](attachment:image.png)

### 基于CNN的MNIST手写数字识别example

![framework](https://camo.githubusercontent.com/afc8cecd033ab300799ceb2bf3b593efa3bda2b7/687474703a2f2f706572736f6e616c2e69652e6375686b2e6564752e686b2f7e63636c6f792f70726f6a6563745f7461726765745f636f64652f696d616765732f666967332e706e67)


```python
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data

mnist = input_data.read_data_sets('../MNIST_data/', one_hot=True)
```

    Extracting ../MNIST_data/train-images-idx3-ubyte.gz
    Extracting ../MNIST_data/train-labels-idx1-ubyte.gz
    Extracting ../MNIST_data/t10k-images-idx3-ubyte.gz
    Extracting ../MNIST_data/t10k-labels-idx1-ubyte.gz



```python
import os
```


```python
os.environ['CUDA_VISIBLE_DEVICES']='0'
```


```python
# parameters
learning_rate = 0.001
num_steps = 500
batch_size = 128
display_step = 10
```


```python
num_input = 784
num_classes = 10
dropout = 0.75
```


```python
X = tf.placeholder(tf.float32, [None, num_input])
Y = tf.placeholder(tf.float32, [None, num_classes])
keep_prob = tf.placeholder(tf.float32)
```


```python
def conv2d(x, W, b, strides=1):
    x = tf.nn.conv2d(x, W, strides=[1, strides, strides, 1], padding="SAME")
    x = tf.nn.bias_add(x, b)
    return tf.nn.relu(x)
```


```python
def maxpool2d(x, k=2):
    return tf.nn.max_pool(x, ksize=[1,k,k,1], strides=[1,k,k,1], padding="SAME")
```


```python
def conv_net(x, weights, biases, dropout):
    x = tf.reshape(x, shape=[-1, 28, 28, 1])
    
    conv1 = conv2d(x, weights['wc1'], biases['bc1'])
    
    conv1 = maxpool2d(conv1, k=2)
    
    conv2 = conv2d(conv1, weights['wc2'], biases['bc2'])
    
    conv2 = maxpool2d(conv2, k=2)
    
    fc1 = tf.reshape(conv2, [-1, weights['wd1'].get_shape().as_list()[0]])
    
    fc1 = tf.add(tf.matmul(fc1, weights['wd1']), biases['bd1'])
    
    fc1 = tf.nn.relu(fc1)
    
    fc1 = tf.nn.dropout(fc1, dropout)
    
    out = tf.add(tf.matmul(fc1, weights['out']), biases['out'])
    
    return out
```


```python
weights = {
    'wc1': tf.Variable(tf.random_normal([5,5,1,32])),
    'wc2': tf.Variable(tf.random_normal([5,5,32,64])),
    'wd1': tf.Variable(tf.random_normal([7*7*64, 1024])),
    'out': tf.Variable(tf.random_normal([1024, num_classes]))
}
```


```python
biases = {
    'bc1': tf.Variable(tf.random_normal([32])),
    'bc2': tf.Variable(tf.random_normal([64])),
    'bd1': tf.Variable(tf.random_normal([1024])),
    'out': tf.Variable(tf.random_normal([num_classes]))
}
```


```python
# Construct model
logits = conv_net(X, weights, biases, keep_prob)
prediction = tf.nn.softmax(logits)
```


```python
# loss_op = tf.reduce_mean(tf.nn.sparse_softmax_cross_entropy_with_logits(logits=logits, labels=tf.argmax(Y, 1)))
loss_op = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(logits=logits, labels=Y))
optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate)
train_op = optimizer.minimize(loss_op)
```


```python
corrected_pred = tf.equal(tf.argmax(prediction, 1), tf.argmax(Y, 1))
accuracy = tf.reduce_mean(tf.cast(corrected_pred, tf.float32))

init = tf.global_variables_initializer()
```


```python
with tf.Session() as sess:
    sess.run(init)
    for step in range(1, num_steps+1):
        batch_x, batch_y = mnist.train.next_batch(batch_size)
        sess.run(train_op, feed_dict={X:batch_x, Y:batch_y, keep_prob:dropout})
        if step % display_step == 0 or step==1:
            loss, acc = sess.run([loss_op, accuracy], feed_dict={X:batch_x, Y:batch_y, keep_prob:1.0})
            print('Step ' + str(step) + ', Minibatch Loss= ' + \
                 "{:.4f}".format(loss) + ', Training Accuracy= ' + \
                 "{:.3f}".format(acc))
    print("Optimization Finished!")
    print("Testing Accuracy:", \
         sess.run(accuracy, feed_dict={X:mnist.test.images[:256], Y:mnist.test.labels[:256], keep_prob:1.0}))
```

    Step 1, Minibatch Loss= 79241.9844, Training Accuracy= 0.117
    Step 10, Minibatch Loss= 26921.6562, Training Accuracy= 0.234
    Step 20, Minibatch Loss= 8619.5146, Training Accuracy= 0.469
    Step 30, Minibatch Loss= 5994.6895, Training Accuracy= 0.641
    Step 40, Minibatch Loss= 3186.5356, Training Accuracy= 0.750
    Step 50, Minibatch Loss= 3672.5601, Training Accuracy= 0.781
    Step 60, Minibatch Loss= 2815.3110, Training Accuracy= 0.828
    Step 70, Minibatch Loss= 2217.8862, Training Accuracy= 0.852
    Step 80, Minibatch Loss= 1313.4218, Training Accuracy= 0.867
    Step 90, Minibatch Loss= 1799.4216, Training Accuracy= 0.875
    Step 100, Minibatch Loss= 2202.5249, Training Accuracy= 0.867
    Step 110, Minibatch Loss= 2464.4990, Training Accuracy= 0.867
    Step 120, Minibatch Loss= 1051.7372, Training Accuracy= 0.898
    Step 130, Minibatch Loss= 1100.5756, Training Accuracy= 0.906
    Step 140, Minibatch Loss= 1442.4780, Training Accuracy= 0.898
    Step 150, Minibatch Loss= 931.7564, Training Accuracy= 0.930
    Step 160, Minibatch Loss= 864.4419, Training Accuracy= 0.906
    Step 170, Minibatch Loss= 1585.5532, Training Accuracy= 0.898
    Step 180, Minibatch Loss= 1027.1390, Training Accuracy= 0.914
    Step 190, Minibatch Loss= 1343.1506, Training Accuracy= 0.898
    Step 200, Minibatch Loss= 183.0806, Training Accuracy= 0.938
    Step 210, Minibatch Loss= 965.0083, Training Accuracy= 0.930
    Step 220, Minibatch Loss= 1409.3810, Training Accuracy= 0.891
    Step 230, Minibatch Loss= 285.1271, Training Accuracy= 0.953
    Step 240, Minibatch Loss= 1168.4979, Training Accuracy= 0.914
    Step 250, Minibatch Loss= 1210.0005, Training Accuracy= 0.883
    Step 260, Minibatch Loss= 566.3414, Training Accuracy= 0.938
    Step 270, Minibatch Loss= 1197.6128, Training Accuracy= 0.906
    Step 280, Minibatch Loss= 1922.6545, Training Accuracy= 0.891
    Step 290, Minibatch Loss= 399.9597, Training Accuracy= 0.977
    Step 300, Minibatch Loss= 1179.4340, Training Accuracy= 0.930
    Step 310, Minibatch Loss= 846.7992, Training Accuracy= 0.938
    Step 320, Minibatch Loss= 418.2365, Training Accuracy= 0.953
    Step 330, Minibatch Loss= 956.3832, Training Accuracy= 0.938
    Step 340, Minibatch Loss= 641.0366, Training Accuracy= 0.938
    Step 350, Minibatch Loss= 1709.7998, Training Accuracy= 0.930
    Step 360, Minibatch Loss= 777.5905, Training Accuracy= 0.922
    Step 370, Minibatch Loss= 611.7239, Training Accuracy= 0.938
    Step 380, Minibatch Loss= 508.9803, Training Accuracy= 0.930
    Step 390, Minibatch Loss= 281.4482, Training Accuracy= 0.969
    Step 400, Minibatch Loss= 1201.5513, Training Accuracy= 0.914
    Step 410, Minibatch Loss= 1000.0850, Training Accuracy= 0.945
    Step 420, Minibatch Loss= 600.8512, Training Accuracy= 0.898
    Step 430, Minibatch Loss= 558.3784, Training Accuracy= 0.922
    Step 440, Minibatch Loss= 646.4320, Training Accuracy= 0.938
    Step 450, Minibatch Loss= 341.6285, Training Accuracy= 0.961
    Step 460, Minibatch Loss= 753.3551, Training Accuracy= 0.922
    Step 470, Minibatch Loss= 492.1851, Training Accuracy= 0.922
    Step 480, Minibatch Loss= 139.8074, Training Accuracy= 0.977
    Step 490, Minibatch Loss= 1322.1320, Training Accuracy= 0.930
    Step 500, Minibatch Loss= 214.8141, Training Accuracy= 0.977
    Optimization Finished!
    Testing Accuracy: 0.96875

