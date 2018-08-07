输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

#### 解法1 递归法  
思路：从叶结点开始，一次向上返回每个节点的深度，直到根节点。简单，就不多说了。  

#### 解法2 基于层序遍历的思想  
使用2个计数器，一个记录本层已经遍历过的节点个数，一个记录本层一共有多少个节点。  

    public int TreeDepth(TreeNode root) {
        if(root==null)
        	return 0;
        int count=0, nextCount=1, depth=0;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while(!queue.isEmpty()){
        	TreeNode top = queue.poll();
        	count++;
        	if(top.left != null)
        		queue.offer(top.left);
        	if(top.right != null)
        		queue.offer(top.right);
        	if(count == nextCount){
        		nextCount = queue.size();
        		count = 0;
        		depth++;
        	}
        }
        return depth;
    }

注意的是LinkedList类实现了Queue接口，因此我们可以把LinkedList当成Queue来用。



## python版

## 基于递归实现

```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def TreeDepth(self, pRoot):
        if not pRoot:
            return 0
        left = self.TreeDepth(pRoot.left)
        right = self.TreeDepth(pRoot.right)
        return max(left, right) + 1

```



## 基于非递归实现

```python
# -*- coding:utf-8 -*-
from collections import deque
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def TreeDepth(self, pRoot):
        #基于层序遍历实现
        if not pRoot:
            return 0
        que = deque()
        que.append(pRoot)
        last, nextlast = pRoot, None
        level = 1
        while len(que) > 0:
            node = que.popleft()
            if node.left:
                nextlast = node.left
                que.append(node.left)
            if node.right:
                nextlast = node.right
                que.append(node.right)
            if node is last:
                if len(que) > 0:
                    level += 1
                    last = nextlast
        return level
```

注意python中队列的使用...