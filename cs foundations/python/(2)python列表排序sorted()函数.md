python列表排序sorted()函数

在Python2.7中, 是cmp参数, 可以传入一个lambda表达式.

lambda表达式, 如果是负值, 表示不要交换, 如果是正值, 表示需要交换...

比如: 

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
# 将2维列表按其中每个列表的长度从大到小排序
lst = []
lst1 = [1,2,3]
lst2 = [2,3,4,5]
lst3 = [3,4,5,6,7]
lst.append(lst1)
lst.append(lst2)
lst.append(lst3)
print lst
lst = sorted(lst, cmp=lambda x,y: len(y)-len(x))   # len(y)-len(x)>0时,也就是y比x长时,表示需要交换..
print lst
```

输出:

```python
[[1, 2, 3], [2, 3, 4, 5], [3, 4, 5, 6, 7]]
[[3, 4, 5, 6, 7], [2, 3, 4, 5], [1, 2, 3]]
```



在python3中, 取消了lambda表达式. 上面的代码在python3中执行会报错:

```python

Traceback (most recent call last):
  File "/usercode/file.py", line 10, in <module>
    print(sorted(lst, cmp=lambda x,y: len(x)-len(y)))
TypeError: 'cmp' is an invalid keyword argument for this function
```

要想在python3中使用类似的功能, 需使用以下方式:

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
from functools import cmp_to_key

lst1 = [[1,2,3]]
lst2 = [[2,3,4,5]]
lst3 = [[3,4,5,6,7]]
lst = lst1 + lst2 + lst3
print(lst)
key = cmp_to_key(lambda x,y:len(y)-len(x))
lst = sorted(lst, key=key)
print(lst)
```

