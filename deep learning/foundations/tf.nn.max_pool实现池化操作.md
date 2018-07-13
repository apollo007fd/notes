# tf.nn.max_pool实现池化操作

max_pool是CNN当中的最大值池化操作. 其用法和卷积很类似

tf.nn.max_pool(value, ksize, strides, padding, name=None)



## value

需要池化的输入, 池化层一般接在卷积层后面, 所以通常是卷积层输出的feature map;

value的shape通常也是[batch, height, width, channels]



## ksize

是池化窗口大小, 一般是[1, height, width, 1]

第1个参数为1, 表示不跳过任何一个训练样本; 第4个参数为1, 表示不跳过任何一个channel. 第2,3个参数意义明显.



## strides

和卷积类似, 窗口在每个维度上滑动的步长, 一般是[1, strides, strides, 1]

第1个参数为1, 表示不跳过任何一个训练样本; 第4个参数为1, 表示不跳过任何一个通道;



## padding

padding通常会有2个选择:

'VALID': 不填充, n*n的图像, 用f\*f去卷积, 输出(n-f+1)\*(n-f+1)图像

'SAME': ~~输出图像要与输入图像的size是一样的, 需要计算填充的行数p~~

​	'SAME': 表示当最后一个池化窗口不满足尺寸要求时, 进行0值填充,使之满足尺寸要求; 参考[2].



## tf.nn.max_pool的返回

返回仍然是一个Tensor, 类型不变, shape仍然是[batch, height, width, channels]这种形式



### 参考:

CSDN上2个高赞博客:

[1]https://blog.csdn.net/mao_xiao_feng/article/details/53453926

[2]https://blog.csdn.net/wuzqChom/article/details/74785643