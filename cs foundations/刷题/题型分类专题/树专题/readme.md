# 树专题索引

[TOC]



# 一些基本概念

### 二叉排序树的定义:

(1)要么是一颗空树

(2)节点的值大于左孩子节点, 小于右孩子节点

(3)左右子树都是二叉排序树



# 剑指offer上关于树的题:

- [x] **7题-重建二叉树:** 给定二叉树前序\中序遍历序列, 构造这颗二叉树
- [x] **8题-二叉树的下一个节点:** 给定一颗二叉树, 以及一个节点, 如何找出中序遍历序列的下一个节点?

- [x] **26题-树的子结构:** 判断一棵树是否是另一颗数的子树

- [x] **27题-二叉树的镜像:** 输入一颗二叉树, 求它的镜像二叉树.

- [x] **28题-对称二叉树:** 判断一棵二叉树树是否是对称二叉树 [**较难**]

- [x] **33题-二叉搜索树的后序遍历序列**
- [x] **34题-二叉树中和为某一值的路径**
- [x] **36题-二叉搜索树与双向链表**  [二叉树的中序遍历-递归-非递归中序遍历]
- [x] **37题-序列化二叉树**
- [x] **54题-二叉搜索树的第k大节点**
- [x] **55题-二叉树的深度**

- [x] **55题-判断二叉树是否是平衡二叉树**



# 用来生成二叉树的函数: 序列化和反序列化二叉树函数

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    i = -1  #静态变量, 用来做当前用来反序列化的节点下标
    # 序列化
    def Serialize(self, root):
        lst, stack, p = [], [], root  # lst用来存放序列化后的字符序列, stack非递归先序遍历所使用的栈.
        while p or len(stack)>0:  # 非递归先序遍历...
            if p:
                lst.append(str(p.val))
                stack.append(p)
                p = p.left
            else:
                lst.append('#')
                p = stack.pop()
                p = p.right
        lst.append('#')  #这一步少不了, 先序序列最后一个节点的空右子树, 需要在这里添加"#"
        return ','.join(lst)
        
    # 反序列化
    def Deserialize(self, s):
        strs = s.split(",")
        head = self.DeserializeHelper(strs)
        return head
    
    def DeserializeHelper(self, strs):
        self.i+=1  # 当前节点值的字符序列下标
        if self.i == len(strs):
            return None # 遍历结束,终止条件
        node = None  # 这一步必不可少, 否则node变成if语句的局部变量了
        if strs[self.i] != '#':
            node = TreeNode(int(strs[self.i]))
            node.left = self.DeserializeHelper(strs)
            node.right = self.DeserializeHelper(strs)
        return node
```



# 题型分类:

## 二叉树的几种遍历

### 中序遍历二叉树-递归

```python
def InOrder(TreeNode T):
    if not T:
        return None
    InOrder(T.left)
    print(T.val)
    InOrder(T.right)
```



### 中序遍历二叉树-非递归

```python
def InOrder(TreeNode T):
    p = T
    stack = []
    while not p or len(stack)!=0:
        if not p:
            stack.append(p)
            p = p.left
        else:
            p = stack.pop()
            print(p.val)
            p = p.right
```



### 先序遍历-递归

```python
    def PreOrder_recursive(root):
        if root:
            print(root.val)
            PreOrder(root.left)
            PreOrder(root.right)
```



### 先序遍历-非递归

```python
    def PreOrder(self, root):
        stack , p= [], root
        while p or len(stack) > 0:
            if p:
                print(p.val)
                stack.append(p)
                p = p.left
            else:
                p = stack.pop()
                p = p.right
```

