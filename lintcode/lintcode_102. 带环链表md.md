# lintcode #
## 102. 带环链表 ##

描述：给定一个链表，判断它是否有环。

样例：给出 -21->10->4->5, tail connects to node index 1，返回 true

挑战：不要使用额外的空间

思路：定义两个快慢指针，快指针一次走两步，慢指针一次走一步，当链表有环时，快指针一定在某个时候追上慢指针，当快指针走完时还没有碰到慢指针，则没有环。

代码如下：
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

<pre>
public class Solution {
    /*
     * @param head: The first node of linked list.
     * @return: True if it has a cycle, or false
     */
    public boolean hasCycle(ListNode head) {
        // write your code here
        ListNode fast = head;
        ListNode slow = head;
        
        if(fast == null || fast.next == null)
            return false;

        //因为要通过fast.next访问fast.next.next,所以fast.next.next不能为空
        while(fast != null && fast.next != null)  
        {
            fast=fast.next.next;
            slow=slow.next;
            
            if(fast == slow)
            {
               return true;
            }
        }
        return false;
    }
}</pre>