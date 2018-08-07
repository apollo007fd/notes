623_Add_One_Row_to_Tree 

给出一棵树的根root, 一个值v和深度d, 需要你在这棵树的第d层添加值为v的节点(设root的深度为1). 

具体规则是: 如果d=1, 那么创建值为v的节点作为根节点, 原root作为左孩子节点.

对于d-1层的每一个非空节点node, 需要添加2个新的节点newleft, newright, 如果node有左孩子left, 那么将left作为newleft的左孩子, 如果node有右孩子, 那么将right作为newright的右孩子.

[传送门](https://leetcode.com/problems/add-one-row-to-tree/description/)



## 非递归的解法, 基于层序遍历的思想.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
import java.util.LinkedList;
class Solution {
    public TreeNode addOneRow(TreeNode root, int v, int d) {
        if(root==null)
            return null;
        if(d==1){
            TreeNode newRoot = new TreeNode(v);
            newRoot.left = root;
            return newRoot;
        }
        LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        int curN = 1, nextN=0;
        for(int i=2; i<d; i++){
            while(curN--!=0){
                TreeNode node = queue.poll();
                if(node.left!=null){
                    queue.offer(node.left);
                    nextN++;
                }
                if(node.right!=null){
                    queue.offer(node.right);
                    nextN++;
                }
            }
            curN = nextN; nextN=0;
        }
        while(!queue.isEmpty()){
            TreeNode node = queue.poll();
            
            TreeNode newleft = new TreeNode(v);
            newleft.left = node.left;
            node.left = newleft;
            
            TreeNode newright = new TreeNode(v);
            newright.right = node.right;
            node.right = newright;
        }
        return root;
    }
}
```



## 递归解法

