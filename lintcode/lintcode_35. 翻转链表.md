# lintcode #
## 35. 翻转链表 ##

描述:翻转一个链表

样例:给出一个链表1->2->3->null，这个翻转后的链表为3->2->1->null

挑战：在原地一次翻转完成

思路：i,m,n分别为链表连续的三个节点，遍历到m，将m指向i，此时链表就断开了，所以必须保存下一个节点
<pre>/**
 * Definition for ListNode
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */</pre>

<pre>
public class Solution {
    /**
     * @param head: n
     * @return: The new head of reversed linked list.
     */
    public ListNode reverse(ListNode head) {
        // write your code here
        ListNode reverseHead =  head ;
        ListNode node  = head;
        ListNode preNode  = null;
        
        while(node != null)
        {
            ListNode nextNode = node.next; //提前保存下一个节点
            if(nextNode == null)
            {
                reverseHead = node;  //找到链表最后一个节点作为新链表的头结点
            }
            node.next = preNode;  //将上一个节点值赋给当前节点的下一个节点，反转链表

            preNode = node;  //将当前节点值赋给上一个节点
            node = nextNode; //从下一个节点开始继续遍历
        }
        return reverseHead;
    }
}</pre>