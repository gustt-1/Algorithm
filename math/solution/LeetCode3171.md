# [LeetCode2154] [将找到的值乘以 2](https://leetcode.cn/problems/keep-multiplying-found-values-by-two/)

## 题目描述
给你一个数组 `nums` 和一个整数 `k` 。你需要找到 `nums` 的一个 子数组 ，满足子数组中所有元素按位或运算 `OR` 的值与 `k` 的 **绝对差** 尽可能 **小**。换言之，你需要选择一个子数组 `nums[l..r]` 满足 `|k - (nums[l] OR nums[l + 1] ... OR nums[r])|` 最小。

请你返回 `最小` 的绝对差值。

子数组 是数组中连续的 `非空` 元素序列。


---


## 输入

> 


## 输出

> 

---

## 我的思路

**思维(LogTrick)**

> 可以先看看暴力解法， 外层枚举右端点 $i$ ，内存循环枚举左端点 $j$ ，我们直接将 $nums[j]$ 修改为**子数组 $i - j$** 的 **或运算的总值**，事件复杂度是 $O(n^2)$ 。

**暴力伪代码**
```
for (int i = 0; i < n; i++) {
    ans = min(ans, abs(k-x));    // 自己单独一个数也是一个子数组
    for (int j = i; j >= 0; j--) {
        nums[j] |= x;
        ans = min(ans, nums[j])
    }
}
```

> 怎么优化呢？对于一个子数组 `i - j` ，我们累加到的答案是 $nums[j]$ ，因为我们是 $j$ 是从 $i$ 开始往前枚举的，所以 nums[j] 即为子数组 `j ~ i-1` 的累计值。可以发现，我们算 `i` 的时候其实 `i - 1` 已经算过了一部分，所以 `j ~ i` 的累计值是在 `j ~ i-1` 加上 $nums[i]$ 的`贡献`。 `nums[j] | x == nums[j]` 如果当前 `i` 位置`对于总值的贡献`为 `0` 那么对于后续的子数组`贡献`也就为 `0` ，那么我们就没必要再枚举剩下的左端点 $j$ 了。

**所以有**
```
for (int i = 0; i < n; i++) {
    ans = min(ans, abs(k-x));
    for (int j = i; j >= 0; j--) {
        if (nums[j] | x == nums[j]) {   // 对于后续的更长的子数组贡献为 0 ，直接退出当前循环
            break
        }
        nums[j] |= x;
        ans = min(ans, nums[j])
    }
}
```

对于复杂度计算，只要我们 `nums[j] | x == nums[j]` 就会退出循环，所以我们最多就是将总和值的`二进制位`全部都填成 `1` ，所以 复杂度是枚举右端点的 $O(n)$ 还有内层最多 $log$ 以 $2$ 为底数每个总值和。所以总的时间复杂度为 $O(nlogn)$


---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func minimumDifference(nums []int, k int) int {
    ans := math.MaxInt
    for i, x := range nums {
        ans = min(ans, abs(k - x))
        for j := i - 1; j >= 0; j-- {
            if nums[j] | x == nums[j] {
                break
            }
            nums[j] |= x
            ans = min(ans, abs(nums[j]-k))
        }
    }
    return ans
}
func abs(x int) int { if x < 0 { return -x }; return x }
```
---

## C++ 代码

```C++
class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        int ans = INT_MAX;
        for (int i = 0; i < nums.size(); i++) {
            int x = nums[i];
            ans = min(ans, abs(x - k));
            for (int j = i - 1; j >= 0 && (nums[j] | x) != nums[j]; j--) {
                nums[j] |= x;
                ans = min(ans, abs(nums[j] - k));
            }
        }
        return ans;
    }
};
```
---
## Python 代码

```Python
class Solution:
    def minimumDifference(self, nums: List[int], k: int) -> int:
        ans = inf
        for i, x in enumerate(nums):
            ans = min(ans, abs(x - k))
            for j in range(i - 1, -1, -1):
                if nums[j] | x == nums[j]:
                    break
                nums[j] |= x
                ans = min(ans, abs(nums[j] - k))
        return ans
```

---

## JavaScript 代码

```JavaScript

```
