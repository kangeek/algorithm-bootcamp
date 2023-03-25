# 数组的度

给定一个非空且只包含非负数的整数数组 nums，数组的 度 的定义是指数组里任一元素出现频数的最大值。

你的任务是在 nums 中找到与 nums 拥有相同大小的度的最短连续子数组，返回其长度。

## 解题思路

对于出现频数的统计可以用哈希表。

与 nums 拥有相同大小的度的最短连续子数组的长度其实就是出现频数最多的数字的第一次和最后一次出现的位置之前的长度。

因此可以：

- 遍历数组，并统计频数，以及记录第一次和最后一次出现的下标；
- 遍历哈希表，找出频数最多的 num，并通过第一次和最后一次出现的下标计算长度。

## 解题代码

```python
class Solution:
    def findShortestSubArray(self, nums: List[int]) -> int:
        num_info = dict()
        for i, num in enumerate(nums):
            if num in num_info:
                num_info[num][0] += 1
                num_info[num][2] = i
            else:
                num_info[num] = [1, i, i]
        
        max_num = 0
        min_len = 0
        for count, first, last in num_info.values():
            if max_num < count:
                max_num = count
                min_len = last - first + 1
            elif max_num == count:
                if min_len > last - first + 1:
                    min_len = last - first + 1
        return min_len
```

## 时空复杂度

时间复杂度：两次遍历遍历数组，因此是 `O(n)`。
空间复杂度：由于需要哈希表保存数据，因此是 `O(n)`。
