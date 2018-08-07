# python中is和==的区别

http://www.iplaypy.com/jinjie/is.html

Python中对象包含的三个基本要素, 分别是:

- id (身份标识)
- python type (数据类型)
- value (值)



is 和 == 的作用都是对对象进行比较判断. 但对对象比较判断的内容并不相同.



==是python标准操作符中的比较操作符, 用来比较判断2个对象的value是否相等; is也被叫做同一性运算符, 这个运算符比较判断的是对象间的唯一身份标识, 也就是id是否相同. 举几个例子:

```
>>> x = y = [4,5,6]
>>> z = [4,5,6]
>>> x == y
True
>>> x == z
True
>>> x is y
True
>>> x is z
False
>>>
>>> print id(x)
3075326572
>>> print id(y)
3075326572
>>> print id(z)
3075328140
```

前3个例子都是True, 最后一个是False, 因为前2个比较明显是True, 第3个比较的是对象的id, 然而x和y对象的id相同. 第4个比较的也是对象的id, 但是x和z对象id不同.

参考:

Python进阶教程 http://www.iplaypy.com/jinjie/