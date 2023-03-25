# 元素和为目标值的子矩阵数量

给出矩阵 matrix 和目标值 target，返回元素总和等于目标值的非空子矩阵的数量。

子矩阵 `x1, y1, x2, y2` 是满足 `x1 <= x <= x2` 且 `y1 <= y <= y2` 的所有单元 `matrix[x][y]` 的集合。

如果 `(x1, y1, x2, y2)` 和 `(x1', y1', x2', y2')` 两个子矩阵中部分坐标不同（如：`x1 != x1`），那么这两个子矩阵也不同。

## 解题思路

**这个题没能独立解出来，看了力扣官方题解，并予以理解。**

枚举子矩阵的上下边界，并计算出该边界内每列的元素和，则原问题转换成了如下一维问题：

给定一个整数数组和一个整数 target，计算该数组中子数组和等于 target 的子数组个数。

力扣上有该问题：[560. 和为K的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)，
首先通过解答该问题，并掌握使用前缀和+哈希表的线性做法。

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        mp = Counter(0)
        pre = count = 0
        for num in nums:
            pre += num
            if (pre - k) in mp:
                count += mp[pre - k]
            mp[pre] += 1
        return count
```

对于每列的元素和 sum 的计算，我们在枚举子矩阵上边界 i 时，初始下边界 j 为 i，此时 sum 就是矩阵第 i 行的元素。
每次向下延长下边界 j 时，我们可以将矩阵第 j 行的元素累加到 sum 中。

```python
class Solution:
    def numSubmatrixSumTarget(self, matrix: List[List[int]], target: int) -> int:
        def subarraySum(nums: List[int], k: int) -> int:
            mp = Counter([0])
            count = pre = 0
            for x in nums:
                pre += x
                if pre - k in mp:
                    count += mp[pre - k]
                mp[pre] += 1
            return count
        
        m, n = len(matrix), len(matrix[0])
        ans = 0
        # 枚举上边界
        for i in range(m):
            total = [0] * n
            # 枚举下边界
            for j in range(i, m):
                for c in range(n):
                    # 更新每列的元素和
                    total[c] += matrix[j][c]
                ans += subarraySum(total, target)
        
        return ans
```

## 时空复杂度

时间复杂度：首先对矩阵的上下边界进行了遍历，也就是 `m*m`，然后嵌套的前缀和的计算又对列进行了遍历，因此是 `O(m*m*n)`。
空间复杂度：感觉也是 `O(m*m*n)`，不过力扣官方是 `O(n)`，不太理解。
