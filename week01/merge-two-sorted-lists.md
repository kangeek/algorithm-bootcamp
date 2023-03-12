# 合并两个有序链表

## 解题思路

`list1` 和 `list2` 直接作为链表的“游标”，指向待合并的节点。然后遍历两个链表的节点：

1. 首先构造一个节点用来作为合并后的链表的头，合并过程中将两个 list 中的节点依次向后接；
1. 遍历过程中，比较两个 list 的处在前面的节点的值，将小的那个节点接在合并后的链表上，
   同时将被“拿走”节点的链表的游标向后移动一位；
1. 重复上一步骤，直到某一个链表全被“拿走”（等同于前面该链表为空的判断），
   另一个链表的剩余节点被接在合并后的链表后面。

## 结题代码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // 构造一个节点用来作为合并后的链表的头
        ListNode mergedList = new ListNode(-101, null);
        // 合并后链表的游标
        ListNode cursor = mergedList;
        while (true) {
            // 如果为空（或没有剩余节点），直接将另一个链表接到合并的链表上
            if (list1 == null) {
                cursor.next = list2;
                break;
            } 
            if (list2 == null) {
                cursor.next = list1;
                break;
            }
            // 判断两个链表前面节点大小，值小的节点被接到合并的链表上，相应的游标向后移动一个节点
            if (list1.val < list2.val) {
                cursor.next = list1;
                list1 = list1.next;
            } else {
                cursor.next = list2;
                list2 = list2.next;
            }
            cursor = cursor.next;
        }
        return mergedList.next;
    }
}
```

## 时空复杂度

时间复杂度：如果 m 和 n 为两个链表的长度，遍历的长度最多不会大于 m + n 次，时间复杂度是 `O(m+n)`。
空间复杂度：需要几个固定个数的变量，空间复杂度为 `O(1)`。
