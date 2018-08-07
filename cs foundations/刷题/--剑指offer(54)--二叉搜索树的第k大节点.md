# 二叉搜索树的第k大节点

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQDpWQGsCalBihoyghS-ICmvrXWhEzK12Luo9ceBglVKc8r4Zhp)

题目：给定一颗二叉搜索树，请找出其中的第k大的结点。例如， 5 / \ 3 7 /\ /\ 2 4 6 8 中，按结点数值大小顺序第三个结点的值为4。
[牛客网链接](https://www.nowcoder.com/practice/ef068f602dde4d28aab2b210e859150a?tpId=13&tqId=11215&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

本题主要考察二叉树中序的写法，二叉搜索树的中序遍历得到一个有小到大的数字序列；

#### 解法1 基于中序遍历非递归实现

    TreeNode KthNode2(TreeNode pRoot, int k)
    {
    	if(pRoot == null || k <= 0)
    		return null;
    	
    	Stack<TreeNode> stack = new Stack<TreeNode>();
        while(pRoot != null || !stack.isEmpty()){
        	while(pRoot != null){
        		stack.push(pRoot);
        		pRoot = pRoot.left;
        	}
        	
        	TreeNode node = stack.pop();
        	if(--k == 0)
        		return node;
        	
        	pRoot = node.right;
        }
        return null;
    }

#### 解法2 基于中序遍历的递归的实现

```java
private static int index = 1;
TreeNode KthNode1(TreeNode pRoot, int k)
{
    index = 1;
    return helper(pRoot, k);
}
TreeNode helper(TreeNode pRoot, int k)
{
    if(pRoot == null)
        return null;
    TreeNode res = helper(pRoot.left, k);
    if (res!=null) return res;
    if(k==index)
        return pRoot;
    index++;
    res = helper(pRoot.right, k);
    if (res!=null) return res;
    return null;
}
```



## python版本-基于非递归中序遍历

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    # 返回对应节点TreeNode
    def KthNode(self, pRoot, k):
        # 中序遍历序列的第k个元素
        stack = []
        p = pRoot
        while p or len(stack)>0:
            if p:
                stack.append(p)
                p = p.left
            else:
                p = stack.pop()
                k -= 1
                if k == 0:
                    return p
                p = p.right
        return None
```

