# lintcode #
## 372. Delete Node in a Linked List ##

描述:
给定一个单链表中的一个等待被删除的节点(非表头或表尾)。请在在O(1)时间复杂度删除该链表节点。

样例:
Linked list is 1->2->3->4, and given node 3, delete the node in place 1->2->4
<pre>
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */</pre>

思路1：常规方法从表头开始遍历，找到待删除节点则删除，时间复杂度O(n)，且该题未给出表头元素。
思路2：将待删除节点的下一个节点的值赋给待删除节点，并且删除下一个节点，时间复杂度O(1)，且不需要给出表头元素（若删除的元素是尾元素则没法通过这种办法删除，则必须从头遍历链表删除）

思路2代码如下：
<pre>
public class Solution {
    /*
     * @param node: the node in the list should be deletedt
     * @return: nothing
     */
    public void deleteNode(ListNode node) {
        // write your code here
        if(node == null || node.next == null)
            return;
        node.val = node.next.val;  //赋值
        node.next = node.next.next; //将待删除的下一个节点删除
    }
}</pre>