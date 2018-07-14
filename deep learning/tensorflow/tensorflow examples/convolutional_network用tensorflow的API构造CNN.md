# 用tensorflow的API构造cnn

[TOC]



# scope

要理解 name_scope 和 variable_scope， 首先必须明确二者的使用目的。我们都知道，和普通模型相比，神经网络的节点非常多，节点节点之间的连接（权值矩阵）也非常多。所以我们费尽心思，准备搭建一个网络，然后有了图1的网络，WTF! 因为变量太多，我们构造完网络之后，一看，什么鬼，这个变量到底是哪层的？？ 

![1531464728101](assets/1531464728101.png)

为了解决这个问题，我们引入了 name_scope 和 variable_scope， 二者又分别承担着不同的责任： 

-  *name_scope:* 为了更好地管理变量的命名空间而提出的。比如在 tensorboard 中，因为引入了 name_scope， 我们的 Graph 看起来才井然有序。 
- *variable_scope:* 大部分情况下，跟 tf.get_variable() 配合使用，实现变量共享的功能。 



## 实验一 变量共享机制

### tf.name_scope()

在Tensorflow中有2中途径生成变量variable, 一种是tf.get_variable(), 另一种是tf.Variable(). 如果在tf.name_scope()的框架下使用这2种方式, 结果会如下:

```python
import tensorflow as tf
import os
os.environ['CUDA_VISIBLE_DEVICES'] ='0'

with tf.name_scope("a_name_scope"):
    initializer = tf.constant_initializer(value=1)
    var1 = tf.get_variable(name='var1', shape=[1], dtype=tf.float32, initializer=initializer)
    var2 = tf.Variable(name='var2', initial_value=[2], dtype=tf.float32)
    var21 = tf.Variable(name='var2', initial_value=[2.1], dtype=tf.float32)
    var22 = tf.Variable(name='var2', initial_value=[2.2], dtype=tf.float32)

with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print(var1.name)
    print(sess.run(var1))
    print(var2.name)
    print(sess.run(var2))
    print(var21.name)
    print(sess.run(var21))
    print(var22.name)
    print(sess.run(var22))
    
输出:
var1:0
[1.]
a_name_scope/var2:0
[2.]
a_name_scope/var2_1:0
[2.1]
a_name_scope/var2_2:0
[2.2]
```

可以看出, 使用tf.Variable()定义的variable, 虽然name都是一样的, 但是为了不重复变量名, tensorflow输出的变量名并不是一样的. 所以本质上, var2, var21, var22并不是一样的变量. 另一方面, 使用tf.get_variable()定义的变量不会被tf.name_scope()当中的名字所影响.



### tf.variable_scope()

```python
import tensorflow as tf
import os
os.environ['CUDA_VISIBLE_DEVICES'] ='0'

with tf.variable_scope('a_variable_scope') as scope:
    initializer = tf.constant_initializer(value=3)
    var3 = tf.get_variable(name='var3', shape=[1], dtype=tf.float32, initializer=initializer)
    scope.reuse_variables()
    var3_reuse = tf.get_variable(name='var3')
    var4=tf.Variable(name='var4', initial_value=[4], dtype=tf.float32)
    var4_reuse = tf.Variable(name='var4', initial_value=[4], dtype=tf.float32)
    
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print(var3.name)
    print(sess.run(var3))
    print(var3_reuse.name)
    print(sess.run(var3_reuse))
    print(var4.name)
    print(sess.run(var4))
    print(var4_reuse.name)
    print(sess.run(var4_reuse))
    
输出:
a_variable_scope/var3:0
[3.]
a_variable_scope/var3:0
[3.]
a_variable_scope/var4:0
[4.]
a_variable_scope/var4_1:0
[4.]
```

可以看出, 如果想要复用variable, 必须使用tf.variable_scope()搭配tf.get_variable()这种方式产生和提取变量, 不想tf.Variable()每次都会产生新的变量, tf.get_variable()如果遇到同样名字的变量时, 它会单纯的提取这个同样名字的变量(避免产生新变量). 而在重复使用的时候, 一定要在代码中强调 `scope.reuse_variables()`, 否则系统将会报错, 以为你只是单纯的不小心重复使用到了一个变量. 



参考:

莫烦tensorflow https://morvanzhou.github.io/tutorials/machine-learning/tensorflow/5-12-scope/



## 实验二 创建变量的3中方式

### 实验目的：探索三种方式定义的变量之间的区别

- tf.placeholder
- tf.Variable
- tf.get_variable



参考:[CSDN高赞](https://blog.csdn.net/Jerr__y/article/details/70809528)

```python
import tensorflow as tf
# 按需分配GPU
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
sess = tf.Session(config=config)
```

### 实验二(1) 验证3中方式定义的变量,命名是否冲突

```python
# 1.placeholder 
v1 = tf.placeholder(tf.float32, shape=[2,3,4])
print(v1.name)

v1 = tf.placeholder(tf.float32, shape=[2,3,4], name='ph')
print(v1.name)

v1 = tf.placeholder(tf.float32, shape=[2,3,4], name='ph')
print(v1.name)

print(type(v1))
print(v1)
```

```
Placeholder:0
ph:0
ph_1:0
<class 'tensorflow.python.framework.ops.Tensor'>
Tensor("ph_1:0", shape=(2, 3, 4), dtype=float32)
```



```python
# 2. tf.Variable()
v2 = tf.Variable([1,2], dtype=tf.float32)
print(v2.name)
v2 = tf.Variable([1,2], dtype=tf.float32, name='V')
print(v2.name)
v2 = tf.Variable([1,2], dtype=tf.float32, name='V')
print(v2.name)
print(type(v2))
print(v2)
```

```
Variable:0
V:0
V_1:0
<class 'tensorflow.python.ops.variables.Variable'>
<tf.Variable 'V_1:0' shape=(2,) dtype=float32_ref>
```



```python
# 3.tf.get_variable() 创建变量的时候必须要提供 name
v3 = tf.get_variable(name='gv', shape=[])
print(v3.name)
v4 = tf.get_variable(name='gv', shape=[2])
print(v4.name)
```

```
gv:0
```



```
---------------------------------------------------------------------------

ValueError                                Traceback (most recent call last)

<ipython-input-5-a7b81c98dc5b> in <module>()
      2 v3 = tf.get_variable(name='gv', shape=[])
      3 print(v3.name)
----> 4 v4 = tf.get_variable(name='gv', shape=[2])
      5 print(v4.name)
```

```
~/.local/lib/python3.5/site-packages/tensorflow/python/ops/variable_scope.py in get_variable(name, shape, dtype, initializer, regularizer, trainable, collections, caching_device, partitioner, validate_shape, use_resource, custom_getter)
   1063       collections=collections, caching_device=caching_device,
   1064       partitioner=partitioner, validate_shape=validate_shape,
-> 1065       use_resource=use_resource, custom_getter=custom_getter)
   1066 get_variable_or_local_docstring = (
   1067     """%s
```

```
~/.local/lib/python3.5/site-packages/tensorflow/python/ops/variable_scope.py in get_variable(self, var_store, name, shape, dtype, initializer, regularizer, reuse, trainable, collections, caching_device, partitioner, validate_shape, use_resource, custom_getter)
    960           collections=collections, caching_device=caching_device,
    961           partitioner=partitioner, validate_shape=validate_shape,
--> 962           use_resource=use_resource, custom_getter=custom_getter)
    963 
    964   def _get_partitioned_variable(self,
```

```
~/.local/lib/python3.5/site-packages/tensorflow/python/ops/variable_scope.py in get_variable(self, name, shape, dtype, initializer, regularizer, reuse, trainable, collections, caching_device, partitioner, validate_shape, use_resource, custom_getter)
    365           reuse=reuse, trainable=trainable, collections=collections,
    366           caching_device=caching_device, partitioner=partitioner,
--> 367           validate_shape=validate_shape, use_resource=use_resource)
    368 
    369   def _get_partitioned_variable(
```

```
~/.local/lib/python3.5/site-packages/tensorflow/python/ops/variable_scope.py in _true_getter(name, shape, dtype, initializer, regularizer, reuse, trainable, collections, caching_device, partitioner, validate_shape, use_resource)
    350           trainable=trainable, collections=collections,
    351           caching_device=caching_device, validate_shape=validate_shape,
--> 352           use_resource=use_resource)
    353 
    354     if custom_getter is not None:
```

```
~/.local/lib/python3.5/site-packages/tensorflow/python/ops/variable_scope.py in _get_single_variable(self, name, shape, dtype, initializer, regularizer, partition_info, reuse, trainable, collections, caching_device, validate_shape, use_resource)
    662                          " Did you mean to set reuse=True in VarScope? "
    663                          "Originally defined at:\n\n%s" % (
--> 664                              name, "".join(traceback.format_list(tb))))
    665       found_var = self._vars[name]
    666       if not shape.is_compatible_with(found_var.get_shape()):
```

```
ValueError: Variable gv already exists, disallowed. Did you mean to set reuse=True in VarScope? Originally defined at:

  File "<ipython-input-5-a7b81c98dc5b>", line 2, in <module>
    v3 = tf.get_variable(name='gv', shape=[])
  File "/home/apollo/.local/lib/python3.5/site-packages/IPython/core/interactiveshell.py", line 2963, in run_code
    exec(code_obj, self.user_global_ns, self.user_ns)
  File "/home/apollo/.local/lib/python3.5/site-packages/IPython/core/interactiveshell.py", line 2903, in run_ast_nodes
    if self.run_code(code, result):
```



```python
print(v3)
```

```
<tf.Variable 'gv:0' shape=() dtype=float32_ref>
```



```python
print(type(v3))
```

```
<class 'tensorflow.python.ops.variables.Variable'>
```



```python
vs = tf.trainable_variables()
print(len(vs))
for v in vs:
    print(v)
```

```
4
<tf.Variable 'Variable:0' shape=(2,) dtype=float32_ref>
<tf.Variable 'V:0' shape=(2,) dtype=float32_ref>
<tf.Variable 'V_1:0' shape=(2,) dtype=float32_ref>
<tf.Variable 'gv:0' shape=() dtype=float32_ref>
```

以上出了tf.placeholder之外,其他方式创建的变量具有相同类型, 而且只有tf.get_variable()创建的变量之间会发生命名冲突.



### 实验二(2) 探索 name_scope 和 variable_scope

实验二目的：熟悉两种命名空间的应用情景。

```python
with tf.name_scope('nsc1'):
    v1 = tf.Variable([1], name='v1')
    with tf.variable_scope('vsc1'):
        v2 = tf.Variable([1], name='v2')
        v3 = tf.get_variable(name='v3', shape=[])
```

```python
print('v1.name',v1.name)
print('v2.name',v2.name)
print('v3.name',v3.name)
```

```
v1.name nsc1/v1:0
v2.name nsc1/vsc1/v2:0
v3.name vsc1/v3:0
```



```python
with tf.name_scope('nsc1'):
    v4 = tf.Variable([1], name='v4')
print('v4.name',v4.name)
```

```
v4.name nsc1_1/v4:0
```

总结: tf.name_scope()并不会对tf.get_variable()创建的变量有任何影响.  
tf.name_scope() 主要是用来管理命名空间的，这样子让我们的整个模型更加有条理。而 tf.variable_scope() 的作用是为了实现变量共享，它和 tf.get_variable() 来完成变量共享的功能。??

1.第一组，用 tf.Variable() 的方式来定义。

```python
def my_image_filter():
    conv1_weights = tf.Variable(tf.random_normal([5,5,32,32]), name='conv1_weights')
    conv1_biases = tf.Variable(tf.zeros([32]), name='conv1_biases')
    conv2_weights = tf.Variable(tf.random_normal([5,5,32,32]), name='conv2_weights')
    conv2_biases = tf.Variable(tf.zeros([32]), name="conv2_biases")
    return None
```

```python
# First call creates one set of 4 variables.
result1 = my_image_filter()
```

```python
# Another set of 4 variables is created in the second call.
result2 = my_image_filter()
```

```python
# 获取所有的可训练变量
vs = tf.trainable_variables()
```

```python
print('There are %d train_able_variables in the Graph.'%len(vs))
for v in vs[8:16]:
    print(v)
```

```
There are 20 train_able_variables in the Graph.
<tf.Variable 'conv1_weights:0' shape=(5, 5, 32, 32) dtype=float32_ref>
<tf.Variable 'conv1_biases:0' shape=(32,) dtype=float32_ref>
<tf.Variable 'conv2_weights:0' shape=(5, 5, 32, 32) dtype=float32_ref>
<tf.Variable 'conv2_biases:0' shape=(32,) dtype=float32_ref>
<tf.Variable 'conv1_weights_1:0' shape=(5, 5, 32, 32) dtype=float32_ref>
<tf.Variable 'conv1_biases_1:0' shape=(32,) dtype=float32_ref>
<tf.Variable 'conv2_weights_1:0' shape=(5, 5, 32, 32) dtype=float32_ref>
<tf.Variable 'conv2_biases_1:0' shape=(32,) dtype=float32_ref>

```

2.第二种方式，用 tf.get_variable() 的方式

```python
# 下面是定义一个卷积层的通用方式
def conv_relu(kernel_shape, bias_shape):
    # Create variable named 'weights'
    weights = tf.get_variable('weights', kernel_shape, initializer=tf.random_normal_initializer())
    # Create variable named "biases".
    biases = tf.get_variable('biases', bias_shape, initializer=tf.random_normal_initializer())
    return None
```

```python
def my_image_filter():
    # 按照下面的方式定义卷积层，非常直观，而且富有层次感
    with tf.variable_scope('conv1'):
        # Variables created here will be named "conv1/weights", "conv1/biases".
        relu1 = conv_relu([5,5,32,32], [32])
    with tf.variable_scope('conv2'):
        # Variables created here will be named "conv2/weights", "conv2/biases".
        return conv_relu([5,5,32,32],[32])
```

```python
with tf.variable_scope('image_filters') as scope:
    result1 = my_image_filter()
    scope.reuse_variables()
    result2 = my_image_filter()
```

```python
vs = tf.trainable_variables()
```

```python
print('There are %d train_able_variables in the Graph: ' % len(vs))
```

```
There are 20 train_able_variables in the Graph: 

```



```python
for v in vs[-4:]:
    print(v)
```

```
<tf.Variable 'image_filters/conv1/weights:0' shape=(5, 5, 32, 32) dtype=float32_ref>
<tf.Variable 'image_filters/conv1/biases:0' shape=(32,) dtype=float32_ref>
<tf.Variable 'image_filters/conv2/weights:0' shape=(5, 5, 32, 32) dtype=float32_ref>
<tf.Variable 'image_filters/conv2/biases:0' shape=(32,) dtype=float32_ref>

```

### 实验二(2) 总结

首先我们要确立一种Graph的思想, 在tensorflow中, 我们定义一个变量,相当于往Graph中添加了一个节点. 和普通的python函数不一样,在一般的函数中, 我们对输入进行处理, 然后返回一个结果,而函数里边定义的一些局部变量我们就不管了. 但是在tensorflow中, 我们在函数里边创建了一个变量, 就是往Graph中添加了一个节点. 出了这个函数后, 这个节点还是存在于Graph中.



## tf.nn.conv2d和tf.layers.conv2d的区别

### 功能相同, 参数有些不同

在tf.nn.conv2d中, filter参数需传入一个Tensor, 也就是传入一个4维的卷积核矩阵:

```
filter: A Tensor. Must have the same type as input. A 4-D tensor of shape [filter_height, filter_width, in_channels, out_channels]
```

在tf.layers.conv2d中,filter参数需要传入一个整数,指定filter的个数:

```
filters: Integer, the dimensionality of the output space (i.e. the number of filters in the convolution).
```

在后者中, 卷积核尺寸由kernel_size指定.



## tf.nn.maxpool和tf.layers.max_pooling2d

### 也是功能相同,参数有些不同

在tf.nn.max_pool中需要指定padding,   而tf.layers.max_pooling2d中padding默认为"valid".

```
tf.layers.max_pooling2d(inputs, pool_size, strides, padding='valid', data_format='channels_last', name=None)
```

```
tf.nn.max_pool(value, ksize, strides, padding, data_format='NHWC', name=None)
```



## tf.contrib.layers.flatten()

### 用来将池化后的输出展开成行向量

这个函数一般用在最后一个卷积层的池化层, 第一个全连接层之间.

假设池化层输出的尺寸是:  shape=[batch_size, height, width, channels]

那么flatten后的输出是:[batch_size, height*width\*channels]





