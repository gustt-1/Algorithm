# [LeetCode2154] [将找到的值乘以 2](https://leetcode.cn/problems/keep-multiplying-found-values-by-two/)

## 题目描述
给你一个整数数组 `nums` ，另给你一个整数 `original` ，这是需要在 `nums` 中搜索的第一个数字。

接下来，你需要按下述步骤操作：

如果在 `nums` 中找到 `original` ，将 `original` 乘以 2 ，得到新 `original`（即，令 `original = 2 * original`）。
否则，停止这一过程。
只要能在数组中找到新 `original` ，就对新 `original` 继续 重复 这一过程。
返回 `original` 的 `最终` 值。


---


## 输入

> 


## 输出

> 

---

## 我的思路

**位运算优化**

> $mask$ 用于来记录出现的 $original * 2^k$

---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func findFinalValue(nums []int, original int) int {
    mask := 0    // 用二进制记录出现过的 original * 2^k 
    // 假设最终 mask = 1111011 那么倒数最低位开始第三位就是要找的答案
    for _, x := range nums {
        k := x/original   // 找到倍数
        if x%original == 0 && k&(k-1) == 0 {   // 如果 k 是 original 的倍数，并且 k 是 2 的幂
            mask |= k   // 记录
        }
    }
    mask = ^mask  // 取反
    return original * (mask & -mask)   // mask & -mask 取最低位的 1
}
```
---

## C++ 代码

```C++
```
---
## Python 代码

```Python
class Solution:
    def findFinalValue(self, nums: List[int], original: int) -> int:
        mask = 0
        for x in nums:
            k, r = divmod(x, original)
            if r == 0 and k&(k-1) == 0:
                mask |= k
        
        mask= ~mask
        return original * (mask & -mask)
```

---

## JavaScript 代码

```JavaScript
/**
 * @param {number[]} nums
 * @param {number} original
 * @return {number}
 */
var findFinalValue = function(nums, original) {
    let mask = 0;
    for (const x of nums) {
        const k = x / original;
        if (x % original == 0 && (k & (k - 1)) == 0) {
            mask |= k;
        }
    }
    mask = ~mask;
    return original * (mask & -mask);
};
```
