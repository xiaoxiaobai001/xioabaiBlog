# lintcode #
## 469. Same Tree ##

描述：检查两棵二叉树是否等价。等价的意思是说，首先两棵二叉树必须拥有相同的结构，并且每个对应位置上的节点上的数都相等。

样例
<pre>
    1             1
   / \           / \
  2   2   and   2   2
 /             /
4             4
就是两棵等价的二叉树。

    1             1
   / \           / \
  2   3   and   2   3
 /               \
4                 4
就不是等价的。
</pre>

//思路：二叉树的操作主要搞清楚递归，想清楚递归的结束条件，判断是不是相同的树，首先比较根节点值是否相同，接下来遍历左子树，遍历右子树，左右子树必须都相同才可以，所以中间有且的条件，最后注意如若一边为空，另一边有子树，则肯定不等。

代码如下：
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
     * @param a: the root of binary tree a.
     * @param b: the root of binary tree b.
     * @return: true if they are identical, or false.
     */
    public boolean isIdentical(TreeNode a, TreeNode b) {
        // write your code here
        if(a == null && b == null)
            return true;
        else if(a == null || b == null)
            return false;   
        else if(a != null && b != null && a.val != b.val)
            return false;
            
        return isIdentical(a.left, b.left)&&isIdentical(a.right, b.right);
    }
}</pre>