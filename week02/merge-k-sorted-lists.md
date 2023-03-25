# 合并 K 个升序链表

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

## 解题思路

采用分治的思想：

对链表数组的处理，可以看做将链表分成两半（链表数组）分别处理。从而可以用递归的方法处理。停止条件是：

- 当链表数组为空时，直接返回 `[]`。
- 当链表数组只有一个链表元素时，直接返回唯一的链表即可。
- 当链表数组又两个链表元素时，对两个链表进行合并。对两个链表的合并其实也可以用递归的方式来处理。
  - 递归比较两个链表的第一个节点，更小的那个节点的 `next` 就是递归的子问题：所在链表的后续和另一个链表的合并。
  - 停止条件是：任意一个链表为空（全部被合并）后，返回另一个链表即可。

## 解题代码

```python
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:

        l = len(lists)
        if l == 0: return None
        if l == 1: return lists[0]

        def mergeTwoLists(list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
            if list1 is None:
                return list2
            elif list2 is None:
                return list1
            elif list1.val < list2.val:
                list1.next = mergeTwoLists(list1.next, list2)
                return list1
            else:
                list2.next = mergeTwoLists(list1, list2.next)
                return list2

        if l == 2:
            return mergeTwoLists(lists[0], lists[1])

        return mergeTwoLists(self.mergeKLists(lists[0:l//2]), self.mergeKLists(lists[l//2:]))
```

## 时空复杂度

时间复杂度：如果链表数组的长度为 m，链表的平均长度为 n，那么对链表数组的处理是 `O(logm)`，而进行链表合并的时间复杂度是 `O(m*n)`，所以是 `O(m*n*logm)`。
空间复杂度：由于递归会使用栈空间，对链表数组来说是 `O(logm)`，对链表合并来说是 `O(n)`，但是总的空间复杂度不清楚应该是多少。
