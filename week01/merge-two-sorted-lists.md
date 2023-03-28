# 合并两个有序链表

## 解题思路（迭代）

`list1` 和 `list2` 直接作为链表的“游标”，指向待合并的节点。然后遍历两个链表的节点：

1. 首先构造一个节点用来作为合并后的链表的头，合并过程中将两个 list 中的节点依次向后接；
1. 遍历过程中，比较两个 list 的处在前面的节点的值，将小的那个节点接在合并后的链表上，
   同时将被“拿走”节点的链表的游标向后移动一位；
1. 重复上一步骤，直到某一个链表全被“拿走”（等同于前面该链表为空的判断），
   另一个链表的剩余节点被接在合并后的链表后面。

## 解题代码（迭代）

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

## 时空复杂度（迭代）

时间复杂度：如果 m 和 n 为两个链表的长度，遍历的长度最多不会大于 m + n 次，时间复杂度是 `O(m+n)`。
空间复杂度：需要几个固定个数的变量，空间复杂度为 `O(1)`。

## 解题思路（递归）

> 补：在学习了第二周的课程，完成[合并 K 个升序链表](./week02/merge-k-sorted-lists.md)的作业时，
再次回到该问题，发现可以使用递归来解决。

由于该问题的每一步都可以看做是一个子问题，因此可以使用递归来解决。具体来说：

- 每个递归周期要解决的问题：比较两个链表的第一个节点，选择更小的那个节点返回；
- 每个递归的子问题：比较之后剩余的两个链表的又是合并两个有序链表的问题；
- 递归终止条件：当某一个链表为空时，返回另一个链表。

## 解题代码（递归）

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        if list1 is None:
            return list2
        elif list2 is None:
            return list1
        elif list1.val < list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
```

## 时空复杂度（递归）

时间复杂度：如果 m 和 n 为两个链表的长度，遍历的长度最多不会大于 m + n 次，时间复杂度是 `O(m+n)`。
空间复杂度：递归的深度最多为 m + n，空间复杂度为 `O(m+n)`。
