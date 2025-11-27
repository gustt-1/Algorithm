# [LeetCode-956] [最高的广告牌](https://leetcode.cn/problems/tallest-billboard/description/)
## 题目描述


> 你正在安装一个广告牌，并希望它高度最大。这块广告牌将有两个钢制支架，两边各一个。每个钢支架的高度必须相等。你有一堆可以焊接在一起的钢筋 `rods` 。举个例子，如果钢筋的长度为 `1、2 和 3` ，则可以将它们焊接在一起形成长度为 `6` 的支架。返回 广告牌的最大可能安装高度 。如果没法安装广告牌，请返回 `0` 。

---

## 我的思路
**0-1背包**

##

题目就是说有一堆钢筋，然后我们选用一些钢筋来搭建广告牌，并且两边高度必须保持一致。每根钢筋只能选用一次，联想到 $0-1$ 背包。关键是我们如何设计 $dp$ 数组，以及如何设计状态。

思路是设计 $f[i][j]$ 表示考虑到第i根钢筋时，两边钢筋差值为 $j$ 的最大钢筋长度，那么我们的答案即为 $f[n - 1][0] / 2$ 表示选择钢筋到最后一根钢筋两边差值为0的最大钢筋长度。因为我们是一直用钢筋堆最大长度，而并没有分开两堆，所以要除以二。我们用第二个维度只是模拟两边钢筋的长度差值。原作者题解[二维动态规划，超详细讲解！](https://leetcode.cn/problems/tallest-billboard/solutions/2428059/er-wei-dong-tai-gui-hua-chao-xiang-xi-ji-l8qi/)

那么如何写状态转移方程。我们考虑第i根钢筋，对于第i根钢筋我们有选或者不选两种选择，并且还要考虑把它放在更长的那一边还是更短的那一堆。

假设第i根钢筋长为 $rods[i]$ ,当前两边高度差为 $d$ 

那么状态转移方程则为 $dp[i][d] = max(dp[i−1][d], dp[i−1][d+rods[i]]+rods[i], dp[i−1][|d − rods[i]|]+rods[i])$

- 1.如果是放在更长的那一边则高度差为 $d + rods[i]$ ,因为更长那么直接加就行。

- 2.如果是更短的那一边那我们要考虑加入了 $rods[i]$ 之后，更短的那一边是否会比更长的那边更长。那我们需要取一个绝对值。
  - （1）如果加入的钢筋长比高度差更短，那么是 $d - rods[i]$ 
  - （2）如果更长了，那么我们把 $rods[i]$ 分成 $d + x$ (因为更长, $d$ 为高度差值， $x$ 为多出的那一部分) $|d - rods[i]| = |d - （d + x）| = |-x| = x$ ,也就是多出的那一部分 $x$ ，也就是新的高度差。

- 3.我们的背包总容量为所有钢筋的长度总和，因为下标从 $0$ 开始，所以我们要多开一个 $（sum+1）$ 。第一层for是遍历所有的钢筋，第二层是遍历背包容量。
- 4$ .由于我们 $dp[i][j]$ 的状态转移只跟 $dp[i-1][]$ 和 $dp[i][]$ （当前层和上一层的状态有关）所以我们可以对 $0-1$ 背包进行空间优化，但是由于 $|d-rods[i]|$ 的不确定性，我们不能只用一个数组从后往前更新，当 $|d - r| < d 时，后面更新的 dp[|d - r|] 会使用到本轮已经更新的 dp[d]，导致错误的状态叠加。所以需要使用两个数组来更新状态，一个用来 $prev$ 保存上层的状态，一个 $dp$ 来更新当前层的状态。
- 5.最后我们返回 $dp[0] / 2$ ，注意 $dp[0]$ 表示考虑完所有的钢筋，使得两堆钢筋长度差为 $0$ 的两堆钢筋最大可以达到的长度，所以我们需要除以 $2$




##
---

## 时间复杂度

$O(n * sum)$ , $n$ 为钢筋数量， $sum$ 为钢筋总长度（背包容量）

---

## 空间复杂度

$O(sum)$  我们只开了两个长度为 $sum$ 的数组

---

## Go 代码

```Go

func tallestBillboard(rods []int) int {
    sum := 0
    for _, r := range rods {
        sum += r
    }

    f := make([]int, sum+1)
    for i := range f {
        f[i] = -int(1e9)
    }
    f[0] = 0

    for _, r := range rods {
        prev := make([]int, len(f))
        copy(prev, f)

        for d := 0; d <= sum-r; d++ {
            if prev[d] < -1e9 {
                continue
            }
            if d+r <= sum {
                f[d+r] = max(f[d+r], prev[d]+r)
            }
            diff := abs(d - r)
            f[diff] = max(f[diff], prev[d]+r)
        }
    }

    return f[0] / 2
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}


```
---

## C++ 代码

```C++

class Solution {
public:
    int tallestBillboard(vector<int>& rods) {
        int s = accumulate(rods.begin(), rods.end(), 0);
        vector<int> f(s + 1, INT_MIN);
        f[0] = 0;

        for (int r : rods) {
            vector<int> prev = f;
            for (int d = 0; d <= s - r; ++d) {
                if (prev[d] == INT_MIN) continue;
                f[abs(d - r)] = max(f[abs(d - r)], prev[d] + r);
                f[d + r] = max(f[d + r], prev[d] + r);
            }
        }

        return f[0] / 2;
    }
};


```
---
## Python 代码

```Python

class Solution:
    def tallestBillboard(self, rods: list[int]) -> int:
        s = sum(rods)
        f = [-float('inf')] * (s + 1)
        f[0] = 0

        for r in rods:
            prev = f[:]
            for d in range(s - r + 1):
                if prev[d] == -float('inf'):
                    continue

                f[d + r] = max(f[d + r], prev[d] + r)

                diff = abs(d - r)
                f[diff] = max(f[diff], prev[d] + r)

        return f[0] // 2

```
