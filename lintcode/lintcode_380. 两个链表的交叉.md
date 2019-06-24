# lintcode #
## 380. 两个链表的交叉 ##

描述：请写一个程序，找到两个单链表最开始的交叉节点。

样例：
<pre>下列两个链表：

A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3</pre>
在节点 c1 开始交叉。

挑战:需满足 O(n) 时间复杂度，且仅用 O(1) 内存。
<pre>
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;      
 *     }
 * }
 */</pre>

//思路1：暴力穷具，双重循环，对第一个链表每一个节点开始遍历，找第二个链表每一个节点是否有当前节点相同，相同则交叉，并返回。时间复杂度O(N^2)

//思路2：若两个单链表若相交，则尾结点一定相同，可先判断两链表是否交叉，若不交叉则直接返回null。然后算链表A的长度，再算出链表B的长度，长的那个链表先把多的长度走完，因为后面交叉则个数一定相同，第一个交叉点不可能存在前面多的长度中，然后接下来判断两个节点是否相同，相同则直接返回。
<pre>
public class Solution {
    /*
     * @param headA: the first list
     * @param headB: the second list
     * @return: a ListNode
     */
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // write your code here
        ListNode tailA = headA;
        ListNode tailB = headB;
        int lenA = 0;
        int lenB = 0;
        
        if(headA == null || headB == null)
            return null;
        while(tailA != null)
        {
            tailA = tailA.next;
            lenA++;
        }
        
        while(tailB != null)
        {
            tailB = tailB.next;
            lenB++;
        }
        
        if(tailA != tailB)
            return null;
        
        ListNode A = headA;
        ListNode B = headB;
        
        if(lenA > lenB)
        {
           int dis = lenA - lenB;
           while(dis != 0)
           {
               A = A.next;
               dis --;
           }

        }else{
           int dis = lenB - lenA;
           while(dis != 0)
           {
               B = B.next;
               dis --;
           }
         
        }
        
        while(A != B)
        {
           A = A.next;
           B = B.next;
        }
        
       return  A;
    }
    
}</pre>