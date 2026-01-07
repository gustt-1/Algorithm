# [LeetCode2555] [两个线段获得的最多奖品](https://leetcode.cn/problems/maximize-win-from-two-segments/description/)
## 题目描述 


> 在 **X** 轴 上有一些奖品。给你一个整数数组 $prizePositions$，它按照 非递减 顺序排列，其中 $prizePositions[i]$ 是第 $i$ 件奖品的位置。数轴上一个位置可能会有多件奖品。再给你一个整数 $k$。你可以同时选择两个端点为整数的线段。每个线段的长度都必须是 $k$。你可以获得位置在任一线段上的所有奖品（包括线段的两个端点）。注意，两个线段可能会有相交。
比方说 $k = 2$，你可以选择线段 $[1, 3]$ 和 $[2, 4]$，你可以获得满足 $1 \le prizePositions[i] \le 3$ 或者 $2 \le prizePositions[i] \le 4$ 的所有奖品 $i$。
请你返回在选择两个最优线段的前提下，可以获得的 最多 奖品数目。

> 


## 我的思路
**枚举/滑动窗口/动态规划**


> 两个线段，枚举右端点时，我们用一个mx数组同时维护左线段长度为k的奖品数量的最大值。期间我们用滑动窗口维护右线段的最大值。

> 维护mx数组，我们用动态规划的思想，我们定义mx[i]为来第i个位置时，所获得的奖品数量的最大值，这是一个子数组（因为线段是连续的）。

> 用来记录答案，其中 $ans = \max(ans, mx[l] + r - l + 1)$，其中 $mx[l]$ 为左线段对的奖品数量的最大值（请注意我们的 $l, r$ 是维护右线段的滑动窗口的左右端点），那么左线段的最大值即是 $mx[l]$（前 $l$ 个位置可以得到的奖品数量最大值）。



> $mx[r+1] = max(mx[r], r - l + 1) $是用来维护左线段的（动态规划思想），类似于[最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)


---

## 时间复杂度

O(n)

---

## 空间复杂度

O(n)

---

## Go 代码

```Go


func maxSumTwoNoOverlap(nums []int, firstLen int, secondLen int) (ans int) {
    fl, sl := firstLen, secondLen
    n := len(nums)
    s := make([]int, n+1)
    for i, x := range nums {
        s[i+1] = s[i] + x
    }
    f := func(fl, sl int) {
        maxsuma := 0
        for i := fl + sl; i <= n; i++ {
            maxsuma = max(maxsuma, s[i-sl]-s[i-sl-fl])
            ans = max(ans, maxsuma + s[i] - s[i-sl])
        }
    }
    f(fl,sl)
    f(sl,fl)
    return
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
    def maxSumTwoNoOverlap(self, nums: List[int], firstLen: int, secondLen: int) -> int:
        n = len(nums)
        s = [0] * (n + 1)
        for i, x in enumerate(nums):
            s[i + 1] = s[i] + x

        ans = 0

        def f(fl, sl):
            nonlocal ans
            maxsuma = 0
            for i in range(fl + sl, n + 1):
                maxsuma = max(maxsuma, s[i - sl] - s[i - sl - fl])
                ans = max(ans, maxsuma + s[i] - s[i - sl])

        f(firstLen, secondLen)
        f(secondLen, firstLen)
        return ans

```
