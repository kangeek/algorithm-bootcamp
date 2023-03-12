# 加一

## 解题思路

思路和平常心算加一操作一样：

1. 从最后一位开始向前遍历，如果不是 `9`，直接加一然后返回就可以了；
1. 如果是 `9`，就向前借一位，即当前数字为 `0`，前一位数字加一；
1. 如果都是 `9`，也就是遍历结束了也没有 `return`，就构造一个 `1000...`
   的数组即可，该数组比原数组多 1 位。

## 结题代码

```java
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
            if (digits[i] < 9) {
                digits[i] += 1;
                return digits;
            } else {
                digits[i] = 0;
            }
        }
        digits = new int[digits.length + 1];
        digits[0] = 1;
        return digits;
    }
}
```

## 时空复杂度

时间复杂度：最多遍历 n 次，是 `O(n)`。
空间复杂度：需要少量固定个数的变量，为 `O(1)`。
