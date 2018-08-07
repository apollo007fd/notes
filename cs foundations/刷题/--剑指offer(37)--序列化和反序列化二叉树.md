# 序列化和反序列化二叉树

![](data:image/jpeg;base64,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)

## 序列化二叉树

很快速地想起了用中序序列和先序序列, 可以恢复一棵二叉树, 然而当这棵树有多个节点值相等时, 就不能使用这种方法了.

19.04

先序遍历序列化二叉树, 遇到空节点, 添加"#", 上面的树, 变成: 1,2,4,#,#,#,3,5,7,#,#,8,#,#,6,#,#

先序遍历可以递归\非递归实现.

## 反序列化二叉树-比较难

反序列化只能递归实现..这道题挺巧妙的..感觉受益颇多..

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

