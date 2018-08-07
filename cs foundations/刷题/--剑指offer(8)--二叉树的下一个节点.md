## --剑指offer(8)--二叉树的下一个节点

题目:给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。 



#### 思路

这个节点可能的情况是:

- 有右子树

  ​	--右子树中序序列第一个

- 没有右子树

  - 这个节点是其父节点的左孩子节点

    --这个节点的父节点

  - 这个节点是其父节点的右孩子节点

    --往上遍历祖父节点:

    - 如果父节点是祖父节点的左孩子

    		--返回祖父节点

    - 如果父节点是祖父节点的右孩子

    ​        --再往上遍历祖祖父节点, 直到根节点.

    ​		如果是根节点的左孩子:

    ​		--返回根节点

    ​		如果是根节点的右孩子:

    ​		--返回null



```java
/*
public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode)
    {
        if(pNode==null)
            return null;
        if(pNode.right != null){
            pNode = pNode.right;
            while(pNode.left != null)
                pNode = pNode.left;
            return pNode;
        }else{
            while(pNode.next!=null){
                if(pNode == pNode.next.left)
                    return pNode.next;
                else
                    pNode = pNode.next;
            }
        }
        return null;
    }
}
```



**python版**

```python
# -*- coding:utf-8 -*-
# class TreeLinkNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#         self.next = None
class Solution:
    def GetNext(self, pNode):
        if not pNode:
            return None
        #右子树是否为空?
        if pNode.right:  
            pNode = pNode.right
            while pNode.left:
                pNode = pNode.left
            return pNode  # 右子树不为空
        else:
            while pNode.next:
                if pNode is pNode.next.left:
                    return pNode.next
                pNode = pNode.next
        return None  # 右子树为空
```

