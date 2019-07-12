# Leetcode 两两交换链表中的节点
## 题目说明：  1->2->3->4   变为   2->1->4->3
方法与思路：首先，设置一个dummy节点，使之指向第一个节点。然后设置指针current指向dummy，当current.next不为空且current.next.next不为空时，
可以进行两个节点之间的交换（例如例子中1跟2的交换），然后将current向后移动两位，继续后面的操作
代码实现：
```java
public ListNode swapPairs(ListNode head){
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode current = dummy;
    while(current.next != null && current.next.next != null){
        swap2(current);
        current = current.next.next;
    }
    return dummy.next;
}

//交换两个节点的值，pre为交换的两个节点之中的第一个节点之前的节点
private void swap2(ListNode pre){
    ListNode dummy = pre.next;
    pre.next = dummy.next;
    dummy.next = dummy.next.next;
    pre.next.next = dummy;
} 
```
