# lintcode #
## 165. 合并两个排序链表 ##

描述：将两个排序链表合并为一个新的排序链表

样例：给出 1->3->8->11->15->null，2->null， 返回 1->2->3->8->11->15->null。

//思路，两个链表都为有序，则每次比较两个链表头元素，取小的加入合并链表的新一个节点。
![](https://i.imgur.com/w4EHJ5P.png)
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
<pre>
public class Solution {
    /**
     * @param l1: ListNode l1 is the head of the linked list
     * @param l2: ListNode l2 is the head of the linked list
     * @return: ListNode head of linked list
     */
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // write your code here
        
        ListNode mergeList = null;
        if(l1 == null)
            return l2;
        else if(l2 == null)
            return l1;
            
        if(l1.val < l2.val){
            mergeList = l1;
            mergeList.next = mergeTwoLists(l1.next,l2);
        }
        else{
            mergeList = l2;
            mergeList.next =  mergeTwoLists(l1,l2.next);
        }
        return mergeList;
    }
}</pre>