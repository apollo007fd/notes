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
    
