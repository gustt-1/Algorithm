# [LeetCode2681] [英雄的力量](https://leetcode.cn/problems/power-of-heroes/description/)

## 题目描述
给你一个下标从 $0$ 开始的整数数组 $nums$ ，它表示英雄的能力值。如果我们选出一部分英雄，这组英雄的 力量 定义为： $i_0, i_1, ... i_k$ 表示这组英雄在数组中的下标。那么这组英雄的力量为 $max(nums[i_0],nums[i_1] ... nums[i_k])2 * min(nums[i_0],nums[i_1] ... nums[i_k])$ 。
请你返回所有可能的 **非空** 英雄组的 **力量** 之和。由于答案可能非常大，请你将结果对 $10^9 + 7$ 取余。

---

## 题目大意



---

## 输入




## 输出

> 

---

## 我的思路

**贡献法**

> 由于排序对题目答案没有影响，我们将 nums 进行升序排序。

> 考虑排序完后为 $a, b, c, d, e$ 。我们假设**当前最大值**为 $d$ ，那么**单独**一个 $d$ 对答案的贡献为 $d^2 * d = d^3$ ，**枚举最小值**为 $a, b, c$ 。以 $a$ 为最小值时， b, c 可选可不选，可选的方案数为 $2^2 = 4$ ，所以以 a 为最小值时对的贡献为 $d^2 * a * 2^2$ ；同样的以 $b$ 为**最小值**时， $c$ 可选可不选，对答案的贡献为  $d^2 * b * 2^1$  ； 以 $c$  为**最小值**时对答案的贡献为 $d^2 * c * 2^0$  。 所以总的贡献为： $d^2 * (a * 2^2 + b * 2^1 + c * 2^0) + d^3$ 。 令 $s = a * 2^2 + b * 2^1 + c * 2^0 , => d^2 * s + d^3$ 。假设以 $e$ 为最大值，可得对答案的贡献为 $e^2 * (a * 2^3 + b * 2^2 + c * 2^1 + d * 2^0) + e^3 => e^2 * 2 * s + d + e^3$ 。可以发现对于当前数为枚举的最大值 $cur$ 那么答案即为 $cur^2*s+cur^3$ , 而 $s$ 即为上一个做的贡献， $s$ 的更新即为 $2 * s + cur$ 。我们每次在答案更新后对 $s$ 进行更新即可为下一个最大值更新答案做准备。


---

## 时间复杂度

$O(nlogn)$ ，其中 $n$ 为 $nums$ 的长度。主要在排序上。

---

## 空间复杂度

$O(1)$

---

## Go 代码

```Go
func sumOfPower(nums []int) (ans int) {
	const mod = 1_000_000_007
	sort.Ints(nums)
	s := 0
	for _, x := range nums {
		ans = (ans + x*x%mod*(x+s)) % mod
		s = (s*2 + x) % mod
	}
	return
}
```
---

## C++ 代码

```C++
class Solution {
public:
    int sumOfPower(vector<int>& nums) {
        const int MOD = 1'000'000'007;
        ranges::sort(nums);
        int ans = 0, s = 0;
        for (long long x : nums) {
            ans = (ans + x * x % MOD * (x + s)) % MOD;
            s = (s * 2 + x) % MOD;
        }
        return ans;
    }
};
```
---
## Python 代码

```Python
class Solution:
    def sumOfPower(self, nums: List[int]) -> int:
        MOD = 1_000_000_007
        nums.sort()
        ans = s = 0
        for x in nums:
            ans = (ans + x * x * (x + s)) % MOD
            s = (s * 2 + x) % MOD
        return ans
```

---

## JavaScript 代码

```JavaScript
```
