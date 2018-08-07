输入一棵二叉树，判断该二叉树是否是平衡二叉树。

#### 解法一 递归法  
如果以当前节点为root的子树是平衡二叉树，则返回每个节点的深度；如果不是，则返回-1；  

    public class Solution {
        public boolean IsBalanced_Solution(TreeNode root) {
            int depth = helper(root);
            return depth != -1;
        }
    
        public int helper(TreeNode node){
            if(node == null)
                return 0;
            int l_depth = helper(node.left);
            int r_depth = helper(node.right);
            if(l_depth == -1 || r_depth == -1)
                return -1;
            if (Math.abs(l_depth - r_depth) > 1)
                return -1;
            return Math.max(l_depth, r_depth) + 1;
        }
    }

这种java解法很巧妙, 当是非平衡树时, 返回-1, 也就相当于能多返回一个状态...



## python 递归解法

由于python可以返回多个值, 所有,python递归解法, 每次可以返回子树是否为平衡树 和 子树的深度; 将求树深和判断是否为平衡树结合起来了.

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def IsBalanced_Solution(self, pRoot):
        rst, depth = self.DepthOfTree(pRoot)
        return rst
        
    def DepthOfTree(self, pRoot):
        if not pRoot:
            return True, 0  # 每次返回2个值
        isBalancedLeft, left = self.DepthOfTree(pRoot.left)
        isBalancedRight, right = self.DepthOfTree(pRoot.right)
        depth = max(left, right) + 1
        isBalanced = abs(left-right) <= 1 and isBalancedLeft and isBalancedRight
        return isBalanced, depth
```

