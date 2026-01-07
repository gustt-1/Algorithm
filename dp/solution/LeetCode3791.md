# [3791] [给定范围内平衡整数的数目](https://leetcode.cn/problems/number-of-balanced-integers-in-a-range/description/)

## 题目描述
给你两个整数 $low$ 和 $high$ 。

如果一个整数同时满足以下 **两个** 条件，则称其为 **平衡** 整数：

它 **至少** 包含两位数字。
**偶数位置上**的数字之和 **等于** **奇数位置上**的数字之和（最左边的数字位置为 $1$ ）。
返回一个整数，表示区间 $[low, high]$（包含两端）内平衡整数的数量。



---

## 题目大意


---

## 输入




## 输出

> 

---

## 我的思路

**数位DP**


- 用 $diff$ 来表示偶数位置上数字和与奇数位置上数字和的差值，符合条件的即为 $diff == 0$ 。枚举当前填的数字 $d$ ， $diff$ + ( $d$ $if$ $i$ % $2$ $else -d$ ) ，其中 $i$ 表示当前填的数字的位置。


---

## 时间复杂度

$O()$ 

---

## 空间复杂度

$O()$

---

## Go 代码

```Go

```
---

## C++ 代码

```C++

```
---
## Python 代码

```Python
class Solution:
    def countBalanced(self, low: int, high: int) -> int:
        if high < 11:
            return 0
        
        low = str(max(low, 11))
        high_s = list(map(int, str(high)))
        n = len(high_s)
        low_s = list(map(int, low.zfill(n)))

        @cache
        def dfs(i: int, diff: int, limit_lo: bool, limit_hi: bool) -> int:
            if i == n:
                return 1 if diff == 0 else 0
            lo = low_s[i] if limit_lo else 0    # 如果前面所有数位都跟n一样，那么当前至少得是 low[i]
            hi = high_s[i] if limit_hi else 9    # 如果前面所有数位都跟n一样，那么当前最多能是 high[i]

            res = 0
            start = lo
            for d in range(start, hi+1):
                res += dfs(i+1, diff+(d if i % 2 else -d), limit_lo and d == lo, limit_hi and d == hi)
            return res

        return dfs(0, 0, True, True)
```

---

## JavaScript 代码

```JavaScript
```
