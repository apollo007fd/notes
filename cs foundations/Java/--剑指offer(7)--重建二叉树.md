--剑指offer(7)--重建二叉树

**题目：**输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建出如下图所示的二叉树并输出它的头结点。　 

![img](https://images0.cnblogs.com/blog2015/381412/201508/182347578161183.jpg) 

#### 思路1

前序序列的第一个结点1, 在中序序列中,可以把结点分为2个部分:

中序序列中结点1之前的,是左子树的节点; 节点1之后的,是右子树的节点;

于是可以形成一个递归建树的过程;



```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        if(pre.length != in.length || pre.length==0)
            return null;
        return helper(pre, 0, pre.length-1, in, 0, in.length-1);
    }
    
    public TreeNode helper(int[] pre, int pstart, int pend, int[] in, int istart, int iend){
        if(pend<pstart || iend < istart)
            return null;
        TreeNode head = new TreeNode(pre[pstart]);
        int lengthleft = 0;
        for(int i=istart; i<=iend; i++){
            if(in[i] != pre[pstart])
                lengthleft++;
            else
                break;
        }
        head.left = helper(pre, pstart+1, pstart+lengthleft, in, istart, istart+lengthleft-1);
        head.right = helper(pre, pstart+lengthleft+1, pend, in, istart+lengthleft+1, iend);
        return head;
    }
}
```

