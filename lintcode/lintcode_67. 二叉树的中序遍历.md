# lintcode #
## 67. 二叉树的中序遍历 ##

描述：给出一棵二叉树,返回其中序遍历

样例:

	给出二叉树 {1,#,2,3},
	
	   1
	    \
	     2
	    /
	   3
	返回 [1,3,2].

挑战：你能使用非递归算法来实现么?

思路1：树采用递归很好解决问题，中序遍历为左根右
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
     * @param root: A Tree
     * @return: Inorder in ArrayList which contains node values.
     */
    List<Integer> list =  new ArrayList<Integer>();
    public List<Integer> inorderTraversal(TreeNode root) {
        // write your code here
         if(root == null)
            return null;
            
        if(root != null)
        {
            inorderTraversal(root.left);
            list.add(root.val);
            inorderTraversal(root.right);
        }

        return list;
    }
}</pre>