## 题目描述
> 给你两个 **非空** 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
>
> 你可以假设除了数字 0 之外，这两个数字都不会以零开头。
>
> **示例1：**
>
> ```
> 输入：l1 = [7,2,4,3], l2 = [5,6,4]
> 输出：[7,8,0,7]
> ```
>
> **示例2：**
>
> ```
> 输入：l1 = [2,4,3], l2 = [5,6,4]
> 输出：[8,0,7]
> ```
>
> **示例3：**
>
> ```
> 输入：l1 = [0], l2 = [0]
> 输出：[0]
> ```


## 方法一（反转链表+逐位计算）
#### 1.解题思路
可以先分别反转两个链表，然后从头节点开始，逐位相加，并连接每次相加之后新建的节点。将最后得到的链表再反转一次，就得到了要求的链表。

#### 2.代码实现
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode h1 = reverse(l1);
        ListNode h2 = reverse(l2);
        int carry = 0;
        ListNode dummy = new ListNode(0);
        ListNode pre = dummy;
        while(h1!=null||h2!=null||carry!=0){
            int val1 = h1==null?0:h1.val;
            int val2 = h2==null?0:h2.val;
            int sum = val1+val2+carry;
            carry = sum/10; 
            ListNode p = new ListNode(sum%10);
            pre.next = p;
            pre = p;
            if(h1!=null){
                h1 = h1.next;
            }
            if(h2!=null){
                h2 = h2.next;
            }
            
        }
        return reverse(dummy.next);
    }

    private ListNode reverse(ListNode head){
        if(head == null || head.next == null) return head;
        ListNode newHead = reverse(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}    
```
#### 3.复杂度分析

 - 时间复杂度：假设两个链表的长度分别为m和n，需要完全遍历两个链表，时间复杂度为O(max(m,n))，两个链表反转的时间复杂度分别为O(m)和O(n)，所以时间复杂度为O(max(m,n))。
 - 空间复杂度：反转的空间复杂度分别为O(m)和O(n)，所以空间复杂度为O(max(m,n))。
