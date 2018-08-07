# 二叉树中和为某一值的路径



题意:

输入一颗二叉树, 和一个整数, 打印出二叉树中节点值的和为输入整数的所有路径. 从树的根节点一直往下, 到叶节点所经过的节点形成一条路径.

![](data:image/png;base64,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)

10.30

算法思想:

如果当前节点为空, 返回一个空链表, 空链表表示没有找到路径.

如果当前节点是叶节点(节点不为空, 但其左、右子树都为空), 且当前节点的值等于expectNumber, 则表示找到一条路径. 将该节点的值, 记录在一个二重list中, (为什么用二重, 便于后续拼接). 这是终止条件, 只有遇到叶节点才会执行.

分别寻找左右子树中的满足要求的路径..然后拼接起来, 将当前节点的val添加到每个一维list的前面.

每次只有遇到一个满足要求的路径, 才回给result列表中加入一个list.

二重列表, 第二重每一个list存放一条路径.

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回二维列表，内部每个列表表示找到的路径
    def FindPath(self, root, expectNumber):
        if not root:
            return [] # 返回空列表,表示遍历到叶节点,没找到路径
        if root and not root.left and not root.right and root.val == expectNumber:
            return [[root.val]]
        left = self.FindPath(root.left, expectNumber-root.val)
        right = self.FindPath(root.right, expectNumber-root.val)
        result = []
        for i in left+right:  # left和right是2个二维列表相加.
            result.append([root.val] + i)  #两个1维列表相加,相当于拼接列表
        return result
```

python list 中的元素也可以是list

两个list相加,  是两个list的元素组成一个list.

```python
>>> lst1 = [1,2,3]
>>> lst2 = [4,5,6]
>>> lst1 + lst2
[1, 2, 3, 4, 5, 6]
>>> lst3 = [[1,2,3]]
>>> lst4 = [[4,5,6]]
>>> lst3+lst4
[[1, 2, 3], [4, 5, 6]]
>>> lst3.append(lst1)
>>> lst3
[[1, 2, 3], [1, 2, 3]]
>>> lst4.append(1)
>>> lst4
[[4, 5, 6], 1]
```

