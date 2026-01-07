# [LeetCode2141] [同时运行 N 台电脑的最长时间](https://leetcode.cn/problems/maximum-running-time-of-n-computers/description/)

## 题目描述
你有 $n$ 台电脑。给你整数 $n$ 和一个下标从 $0$ 开始的整数数组 `batteries` ，其中第 `i` 个电池可以让一台电脑 运行 $batteries[i]$ 分钟。你想使用这些电池让 **全部** `n` 台电脑 **同时** 运行。

一开始，你可以给每台电脑连接 至多一个电池 。然后在任意整数时刻，你都可以将一台电脑与它的电池断开连接，并连接另一个电池，你可以进行这个操作 任意次 。新连接的电池可以是一个全新的电池，也可以是别的电脑用过的电池。断开连接和连接新的电池不会花费任何时间。

注意，你不能给电池充电。

请你返回你可以让 $n$ 台电脑同时运行的 **最长** 分钟数。

---

## 题目大意



---

## 输入

> $1 <= n <= batteries.length <= 10^5$

> $1 <= batteries[i] <= 10^9$


## 输出

> 

---

## 我的思路

**二分答案**

> 可以通过数据范围猜测解题方法为二分答案。

> 对于所有的电池，我们用 `sum` 来统计所有的 $min(x, batteries[i])$ ，其中 x 是二分猜的答案。

> 可以知道我们能使得所有电脑可以同时运行的充分条件是 $n * x <= sum$ ，即所有电脑可以同时运行可以得出 $n * x <= sum$ 。那么 $n * x <= sum$ 可不可以反过来推出呢？即验证必要性。

> 因为我们的 $sum$ 是 $min(x, batteries[i])$ 累加起来的，所以**大于**我们`二分猜的答案` $x$ 的会变成 $x$ ，**小于则不变**。给电脑供电我们可以将电池**分成两部分**，一部份是**大于** $x$ 的，由于我们猜的答案是 $x$ ，所以**最多**供电 $x$ 的时间，那么我们可以将比 $x$ 大的电池给一个电脑上持续供电（多出的电量也用不到了，因为`时间最长为 x` ）。
**小于** $x$ 的呢？ $sum = sum1 + sum2 = n * x$ ，假设 $sum1$ 是大于的哪些电池，由于 $sum$ 累加取的是 $min(x, batteries[i])$ ，那么 $sum1$ 一定是 $x$ 的倍数也就是 $k * x$ ，那么小于 $x$ 的 $sum2$ 就是 $(n-k)*x$ 。那么 $n - k$ 就是剩下还未供电的数量。我们剩下的电池电量是小于 $x$ 的，从上面的可以得出电池数量一定是大于电脑数量的。我们可以选择一部分电池用于给剩下的第一台电脑供电，如果多出的电量我们就又给剩下的第一台电脑供电，以此类推，这是一个又一个的子问题。所以最少需要 $sum2 * (n - k)$ 的电量。又因为我们的电池电量是小于 $x$ 的，所以`不存在同时供电`的情况。

> 例如：我们就假设剩余电池给 $2$ 台电脑供电，有 $3$ 个电池， $x$ 为 $5$ ，电量用数量表示。那么可以每个 $x$ 划分一次（用@标出），可以先让 $2$ 个电池电脑 $1$ 供电，等到给 $1$ 供电的那部分供应完成后就给电脑 $2$ 供电。这样是一定不会存在一个电池给多个电脑供电的情况。

  $[111][22@22][333@]$

**想象我们把其余电池连成一条线，每隔 x 切一刀，每段分给一台电脑**



---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func maxRunTime(n int, batteries []int) int64 {
    total := 0
    for _, b := range batteries {
        total += b
    }
    return int64(sort.Search(total/n + 1, func(t int) bool {
        sum := 0
        for _, b := range batteries {
            sum += min(b, t)
        }
        return n * t > sum
    })) - 1
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
    def maxRunTime(self, n: int, batteries: List[int]) -> int:
        r = sum(batteries) // n
        check = lambda t: n * (t + 1) > sum(min(b, t + 1) for b in batteries)
        return bisect_left(range(r), True, key=check)
```

---

## JavaScript 代码

```JavaScript
/**
 * @param {number} n
 * @param {number[]} batteries
 * @return {number}
 */
var maxRunTime = function(n, batteries) {
    let r = Math.floor(batteries.reduce((s, x) => s + x, 0) / n) + 1;
    let l = -1;
    const check = x => {
        let sum = 0;
        for (const b of batteries) {
            sum += Math.min(x, b);
        }
        return n * x <= sum;
    }
    while (l <= r) {
        let m = Math.floor((l + r) / 2);
        let total = 0;
        for (let cap of batteries) {
            total += Math.min(cap, m);
        }
        if (total >= n * m) {
            ans = m;
            l = m + 1;
        } else {
            r = m - 1;
        }
    }
    return l - 1;
};
```
