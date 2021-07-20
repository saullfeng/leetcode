### 迭代法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(0);
        ListNode pre=head;
        while(l1!=null&&l2!=null)
        {
            if(l1.val<l2.val)
            {
                pre.next=l1;
                l1=l1.next;
            }else
            {
                pre.next=l2;
                l2=l2.next;
            }
            pre=pre.next;
        }
        pre.next=l1==null?l2:l1;
        return head.next;
    }
}
```

### 递归法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null)
        {
            return l2;
        }
        if(l2==null)
        {
            return l1;
        }
        if(l1.val<l2.val)
        {
           //取消l1指针原指向，指向一个新的两个链表，具体指向等返回值
            l1.next=mergeTwoLists(l1.next,l2);
            //此返回值在递归返回时起指向作用
            return l1;
        }else
        {
            l2.next=mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
}
```

