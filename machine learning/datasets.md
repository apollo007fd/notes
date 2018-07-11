# datasets and their usage

MNIST数据集:

tensorflow自带的MNIST数据集的导入:

```python
from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets("./mnist_data/", one_hot=True)
```

