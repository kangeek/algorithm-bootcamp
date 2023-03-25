# 子域名访问计数

## 解题思路

对于输入数组中的每一个元素进行遍历：

- 用空格将访问次数和访问域名拆分；
- 用 `.` 将访问域名拆分为多级；
- 每一级域名保存在哈希表中，哈希表的 key 是各级域名，value 加上访问次数，Python 中有现成的类 `collections.Counter`。

最后按格式输出。

## 解题代码

```python
class Solution:
    def subdomainVisits(self, cpdomains: List[str]) -> List[str]:
        result = Counter()
        for cpdomain in cpdomains:
            count, domain = cpdomain.split()
            count = int(count)
            result[domain] += count
            while '.' in domain:
                domain = domain[domain.index('.') + 1:]
                result[domain] += count
        return [f"{domain} {count}" for count, domain in result.items()]
```

## 时空复杂度

时间复杂度：如果输入数组长度为 n，因为需要遍历一遍，并且针对每个元素的处理次数是有限的，因此是 `O(n)`。
空间复杂度：由于需要一个哈希表来记录数据，因此是 `O(n)`。
