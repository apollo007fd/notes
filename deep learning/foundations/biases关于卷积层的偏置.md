对于一个卷积层, 在卷积操作之后, 需要加上本卷积层的biases作为本卷积层的输出, 

```
conv = tf.nn.conv2d(x, W, strides=[1, height_strides, width_strides, 1], padding="SAME"); 
out = tf.add(conv, biases)
```

W是卷积核, 具有shape: [filter_height, filter_width, in_channels, out_channels].

padding="SAME"表示要对输入图像进行填充, 使卷积输出和输入图像尺寸相同;

特别注意, biases的个数与卷积核的out_channels相同.