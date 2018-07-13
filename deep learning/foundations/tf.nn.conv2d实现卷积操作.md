# tf.nn.conv2d是怎样实现卷积的？



tf.nn.conv2d(input, filter, strides, padding, use_cudnn_on_gpu=None, name=None) 



## input

指需要做卷积的输入图像, 它要求是Tensor, 具有[batch, height, width, channels]这样的shape. 具体含义是[训练时一个batch的图片数量, 图片高度, 图片宽度, 图像通道数], 注意这是一个4维的Tensor, 要求类型为float32和float64其中之一.



## filter

相当于CNN的卷积核, 它要求是一个Tensor, 也具有[height, width, in_channels, out_channels] 这样的shape, 具体含义是[卷积核的高度, 卷积核的宽度, 图像通道数, 卷积核个数], 要求类型与参数input相同, 有一个地方需要注意, 第3维in_channels, 就是参数input的第4维.



## strides

卷积时在图像每一维的步长, 这是一个一维的向量, 长度4.

[batch_stride, height_stride, width_stride, channel_stride], 分别是样本步长, 在图像高度上的步长, 在图像宽度上的buchang, 通道步长. 

一般为[1, height_stride, width_strde, 1], 即不跳过每一个样本和通道.



## padding

string类型的量, 只能是'SAME'或'VALID'.



## **use_cudnn_on_gpu** 

是否用gpu加速, 默认为True



## return

返回的是一个Tensor, 也就是平常说的feature map, 具有[batch_size, height, width, channel]的shape, 分别是[一个batch的图像数量, 图像高度, 图像宽度, 通道数],  其中channel和**filter**的第4个参数相同.



参考:

https://blog.csdn.net/mao_xiao_feng/article/details/78004522  (CSDN高赞)



