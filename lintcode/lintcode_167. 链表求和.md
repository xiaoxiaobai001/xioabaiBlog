# lintcode #
## 167. 链表求和 ##

描述：你有两个用链表代表的整数，其中每个节点包含一个数字。数字存储按照在原来整数中相反的顺序，使得第一个数字位于链表的开头。写出一个函数将两个整数相加，用链表形式返回和。

样例：给出两个链表 3->1->5->null 和 5->9->2->null，返回 8->0->8->null


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

//思路：当链表都不为空时，将链表节点的每个值相加得新链表节点的值，注意进位，当链表某一个为空时，直接将该链表节点的值加入新链表中。

// talk is cheap , show me the code.
//代码如下：
<pre>
public class Solution {
    /**
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
    public ListNode addLists(ListNode l1, ListNode l2) {
        // write your code here
        //特殊处理，两个链表都为空，直接返回null
        if(l1 == null && l2 == null)
            return null;
        //新建头结点，方便操作   
        ListNode list = new ListNode(0);
        //需要移动指针，为了最后返回，保存头结点
        ListNode head = list;
        //进位
        int carry = 0;
            
        while(l1!=null && l2!=null)
        {
            //值需要加上一个进位
            int sum = l1.val + l2.val + carry;
            head.next = new ListNode(sum % 10);
            carry = sum / 10;
            l1 = l1.next;
            l2 = l2.next;
            head  = head.next;
        }
        
         while(l1!=null)
        {
    
            int sum = l1.val + carry;
            head.next = new ListNode(sum % 10);
            carry = sum / 10;
            l1 = l1.next;
            head  = head.next;
        }
        
         while(l2!=null)
        {

            int sum = l2.val + carry;
            head.next = new ListNode(sum % 10);
            carry = sum / 10;
            l2 = l2.next;
            head  = head.next;
        }
        //当链表为空，还有进位时，增加一个节点
        if(carry ==1)
        {
           head.next = new ListNode(1);
        }
        //最终需返回新链表第一个节点
        return list.next; 
    }
}</pre>