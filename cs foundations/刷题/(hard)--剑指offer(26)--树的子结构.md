# 树的子结构

输入2颗二叉树A和B, 判断B是不是A的子结构. 

![](https://images2018.cnblogs.com/blog/1307402/201803/1307402-20180318105728050-1624363603.png)

**思想**1

递归解法, 遍历二叉树, 假设当前遍历的节点作为树根节点, 可以跟子树匹配..

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def HasSubtree(self, pRoot1, pRoot2):
        # write code here
        result = False
        if pRoot1 and pRoot2:   //判断指针不为None的简洁写法
            if pRoot1.val == pRoot2.val:
                result = self.judge(pRoot1, pRoot2)
            if not result:
                result = self.HasSubtree(pRoot1.left, pRoot2)
            if not result:
                result = self.HasSubtree(pRoot1.right, pRoot2)
        return result
        
    def judge(self, A, B):
        if not B:
            return True
        if not A:
            return False
        if A.val != B.val:
            return False
        return self.judge(A.left, B.left) and self.judge(A.right, B.right)
```



