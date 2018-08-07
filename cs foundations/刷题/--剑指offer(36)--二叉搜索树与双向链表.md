# 二叉搜索树与双向链表

15.03

输入一颗二叉搜索树, 将该二叉搜索树转换成一个排序的双向链表. 要求不能创建任何新的节点. 只能调整树中节点指针的指向. 比如, 输入图中左边的二叉搜索树, 则输出转换后排序双向链表.

![](http://wiki.jikexueyuan.com/project/for-offer/images/38.png)

17.32

### 算法思想:

中序遍历二叉树, 边遍历, 边修改指针..



### 这是递归的中序遍历..还有非递归的中序遍历.

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Convert(self, pRootOfTree):
        if not pRootOfTree:
            return None
        lst = []
        self.ConvertCore(pRootOfTree, lst)
        lst[-1].right = None
        return lst[0]
    
    def ConvertCore(self, pRootOfTree, lst):
        if not pRootOfTree:
            return None
        self.ConvertCore(pRootOfTree.left, lst)
        lst.append(pRootOfTree)
        if len(lst)>1:
            lst[-2].right = lst[-1]
            lst[-1].left = lst[-2]
        self.ConvertCore(pRootOfTree.right, lst)
```



### 非递归的中序遍历

```python
def Convert(self, pRootOfTree):
    if not pRootOfTree:
        return None
    p, stack, lst= pRootOfTree,[],[]
    while p or len(stack)>0:
        if p:
            stack.append(p)
            p = p.left
        else:
            p = stack.pop()
            lst.append(p)
            if len(lst) >= 2:
                lst[-2].right = lst[-1]
                lst[-1].left = lst[-2]
            p = p.right
	lst[-1].right = None
	return lst[0]
```

