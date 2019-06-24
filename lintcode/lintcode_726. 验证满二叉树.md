# lintcode #
## 726. 验证满二叉树 ##

描述：如果一棵二叉树所有节点都有零个或两个子节点, 那么这棵树为满二叉树. 反过来说, 满二叉树中不存在只有一个子节点的节点.
<pre>
满二叉树
      1
     / \
    2   3
   / \
  4   5
	
不是一棵满二叉树
      1
     / \
    2   3
   / 
  4   
</pre>

样例:

给出树 {1,2,3}, 返回 true
给出树 {1,2,3,4}, 返回 false
给出树 {1,2,3,4,5}, 返回 true
输入测试数据 (每行一个参数)

<pre>
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */</pre>
<pre>
public class Solution {
    /**
     * @param root: the given tree
     * @return: Whether it is a full tree
     */
     //思路1：满二叉树除了叶子节点外，其他节点都有两个孩子
     
    public boolean isFullTree(TreeNode root) {
        // write your code here
        if(root == null)
            return true;
         
        if(root.left == null && root.right == null){
            return true;
        }else if(root.left == null){
            return false;
        }else if(root.right == null){
            return false;
        }

        return isFullTree(root.left)&&isFullTree(root.right);
    }
    
    //思路2：计算二叉树节点深度，计算所以孩子节点的数目等不等于2^k -1，仅提供思路，觉得没有必要把题复杂化。
}</pre>