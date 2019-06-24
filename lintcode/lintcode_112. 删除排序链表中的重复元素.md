# lintcode #
## 112. 删除排序链表中的重复元素 ##

描述：给定一个排序链表，删除所有重复的元素每个元素只留下一个。

样例：

给出 1->1->2->null，返回 1->2->null

给出 1->1->2->3->3->null，返回 1->2->3->null
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

//思路：已排序的链表，可以依据其特点，只需判断下一个是否与上一个节点相同，有则删除
<pre>
public class Solution {
    /**
     * @param head: head is the head of the linked list
     * @return: head of linked list
     */
     /*
     
     //1.加头结点
    public ListNode deleteDuplicates(ListNode head) {
        // write your code here
        ListNode pre = new ListNode(0);
        pre.next = head;
        head = pre;
        while(head.next != null && head.next.next != null)
        {
            if(head.next.val == head.next.next.val)
            {
                head.next = head.next.next;
            }
            else
                head= head.next;
        }
        return pre.next;
    }*/
    
    
    //2.不需要加头结点，因为重复元素只留一个，故首节点不会被删除
    public ListNode deleteDuplicates(ListNode head) {
        // write your code here
        ListNode pre = head;
        while(head!= null && head.next != null)
        {
            if(head.val == head.next.val)
            {
                head.next = head.next.next;
            }
            else
                head= head.next;
        }
        return pre;
    }
}</pre>