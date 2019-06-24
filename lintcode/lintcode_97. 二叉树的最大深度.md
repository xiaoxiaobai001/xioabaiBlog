# lintcode #
## 97. 二叉树的最大深度#

描述:

给定一个二叉树，找出其最大深度。(二叉树的深度为根节点到最远叶子节点的距离。)

样例:

	给出一棵如下的二叉树:
	
	  1
	 / \ 
	2   3
	   / \
	  4   5
	这个二叉树的最大深度为3.

思路：对于树而言，很容易想到递归，树的深度就等于max{左子树的深度，右子树的深度} + 1；
左子树的深度又可以看做以左孩子节点为根节点，右子树的深度又可以看做以右孩子节点为根节点，递归可得最终结果。

代码如下：
<pre>/**
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
     * @param root: The root of binary tree.
     * @return: An integer
     */
    public int maxDepth(TreeNode root) {
        // write your code here
        int maxDepLeft = 0;
        int maxDepRight = 0;
        
        if(root == null)
            return 0;
            
        if(root.left != null)
        {
           maxDepLeft = maxDepth(root.left);
        }
        
        if(root.right != null)
        {
           maxDepRight= maxDepth(root.right);
        }
        
        return (maxDepLeft>maxDepRight)?(maxDepLeft+1):(maxDepRight+1);
        
    }
}</pre>