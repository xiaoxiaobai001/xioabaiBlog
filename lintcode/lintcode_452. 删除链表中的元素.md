# lintcode #
## 452. 删除链表中的元素 ##

描述：删除链表中等于给定值val的所有节点。

样例：给出链表 1->2->3->3->4->5->3, 和 val = 3, 你需要返回删除3之后的链表：1->2->4->5。
<pre>
/**
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

//思路：遍历链表，找到当前节点与给定值val 相同的节点删除，指向下一节点，注意头首点若为给定值val 时需特殊处理，增加头节点统一操作，无须特殊考虑第一个节点。

<pre>
public class Solution {
    /**
     * @param head: a ListNode
     * @param val: An integer
     * @return: a ListNode
     */
    public ListNode removeElements(ListNode head, int val) {
        // write your code here
        if(head == null)
        {
           return head;
        }
        //增加头结点，方便操作
        ListNode  pre = new ListNode(0);
        pre.next = head;
        //用head指向下一节点等操作，保存头节点
        head = pre;

        while(head.next != null)
        {
            
            if(head.next.val == val)
            {
                head.next = head.next.next;
            }
            else
            {
               head = head.next;
            }
        }
        
      return pre.next;

    }
}</pre>
