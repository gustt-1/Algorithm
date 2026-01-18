# [3811] [交替按位异或分割的数目](https://leetcode.cn/problems/number-of-alternating-xor-partitions/description/)

## 题目描述
给你一个整数数组 `nums` 以及两个 互不相同 的整数 `target1` 和 `target2` 。

`nums` 的一个 分割 是指将其划分为一个或多个 **连续且非空** 的块，这些块在不重叠的情况下覆盖整个数组。

如果一个分割中各块元素的 **按位异或** 结果在 `target1` 和 `target2` 之间 **交替** 出现，且以 `target1` **开始**，则称该分割是 **有效的**。

形式上，对于块 `b1, b2, ...` ：

$XOR(b1) = target1$

$XOR(b2) = target2$ （如果存在）

$XOR(b3) = target1$ ，以此类推。

返回 `nums` 的**有效分割方案数**，结果对 $10^9 + 7$ 取余。

注意： 如果单个块的 **按位异或** 结果等于 `target1`，则该分割也是有效的。


---

## 题目大意


---

## 输入




## 输出

> 

---

## 我的思路

**合法子序列DP**


- 定义 `f1[s]` 表示前缀异或和为 $s$ 且最后一个块的按位异或结果为 `target1` 的有效分割方案数。
- 定义 `f2[s]` 表示前缀异或和为 $s$ 且最后一个块的按位异或结果为 `target2` 的有效分割方案数。
- 用 pre 来表示来到当前位置的所有前缀异或和。

> 初始化 `f1[0] = 0` 前缀异或和为 $0$ 的方案数为 $0$ ，因为要从 $target1$ 开始。`f2[0] = 1` 。相当于在第一段前面有一个空前缀，异或和为 $0$ 。这样我们计算第一段时，就可以让第一段的方案数是 $1$ 。

> 去掉以 $target1$ 的前缀即为 $pre \oplus target1$ ，所以 $f2[pre]$ 可以从 $f2[pre \oplus target1]$ 转移过来，那么转移方程为： $f2[pre] = (f2[pre] + f2[pre \oplus target1]) \mod (10^9 + 7)$ 。

> 同样的，去掉以 $target2$ 的前缀即为 $pre \oplus target2$ ，所以 $f1[pre]$ 可以从 $f1[pre \oplus target2]$ 转移过来，那么转移方程为： $f1[pre] = (f1[pre] + f1[pre \oplus target2]) \mod (10^9 + 7)$ 。


---

## 时间复杂度

$O()$ 

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func alternatingXOR(nums []int, target1 int, target2 int) int {
    const mod = 1_000_000_007
    pre := 0
    f1 := map[int]int{}   // f[s] 表示分割异或和为 s 的前缀且最后一段是 target1 的方案数
	f2 := map[int]int{0: 1}

    for i, x := range nums {
        pre ^= x
        ls1 := f2[pre^target1]
        ls2 := f1[pre^target2]
        if i == len(nums) - 1 {
            return (ls1 + ls2) % mod
        }
        f1[pre] = (f1[pre] + ls1) % mod
        f2[pre] = (f2[pre] + ls2) % mod
    }
    return 1
}
```
---

## C++ 代码

```C++
class Solution {
public:
    int alternatingXOR(vector<int>& nums, int target1, int target2) {
        constexpr int mod = 1'000'000'007;
        unordered_map<int, int> f1;
        unordered_map<int, int> f2 = {{0, 1}};
        int pre = 0;
        for (int i = 0; ; i++) {
            pre ^= nums[i];
            int ls1 = f2[pre ^ target1];
            int ls2 = f1[pre ^ target2];
            if (i == nums.size() - 1) {
                return (ls1 + ls2) % mod;
            }
            f1[pre] = (f1[pre] + ls1) % mod;
            f2[pre] = (f2[pre] + ls2) % mod;
        }
    }
};
```
---
## Python 代码

```Python
class Solution:
    def alternatingXOR(self, nums: List[int], target1: int, target2: int) -> int:
        mod = 1_000_000_007
        f1 = defaultdict(int)
        f2 = defaultdict(int)
        f2[0] = 1
        pre = 0
        for i, x in enumerate(nums):
            pre ^= x
            ls1 = f2[pre ^ target1]
            ls2 = f1[pre ^ target2]
            if i == len(nums) - 1:
                return (ls1 + ls2) % mod
            f1[pre] = (f1[pre] + ls1) % mod
            f2[pre] = (f2[pre] + ls2) % mod
```

---

## JavaScript 代码

```JavaScript
```
