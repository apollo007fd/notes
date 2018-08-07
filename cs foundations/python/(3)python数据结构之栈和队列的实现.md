# python数据结构之栈和队列的实现

# 栈

在python中用列表list来实现栈

```python
>>> stack = [3,4,5]
>>> stack.append(6)
>>> stack.append(7)
>>> stack
[3, 4, 5, 6, 7]
>>> stack.pop()
7
>>> stack
[3, 4, 5, 6]
>>> stack.pop()
6
>>> stack
[3, 4, 5]
```

# 队列

collections.deque实现了双端队列

```python
>>> from collections import deque
>>> queue = deque(['Eric', 'Join', 'Michael'])
>>> queue.append('Terry')
>>> queue.append('Graham')
>>> queue
deque(['Eric', 'Join', 'Michael', 'Terry', 'Graham'])
>>> queue.popleft()
'Eric'
>>> queue.popleft()
'Join'
>>> queue
deque(['Michael', 'Terry', 'Graham'])
```

deque除了append(尾部入队)\popleft(前端出队)方法, 还有appendleft(前端添加)\pop(末尾出队).