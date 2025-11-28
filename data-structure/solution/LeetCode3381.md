# [LeetCode-3381] [长度可被 K 整除的子数组的最大元素和](https://leetcode.cn/problems/maximum-subarray-sum-with-length-divisible-by-k/description/)

## 题目描述
给你一个整数数组 `nums` 和一个整数 `k` 。

返回 `nums` 中一个 `非空子数组` 的 `最大` 和，要求该子数组的长度可以 `被 k 整除` 。

---

## 题目大意


---

## 输入

> 


## 输出

> 

---

## 我的思路

**前缀和/枚举右维护左**

枚举右端点，由于前缀和计算子数组和为 `sum[r] - sum[l]` ，那么我们维护一个`最小的`左端点。由于 $r - l$ 要能被 $k$ `整除`，那么我们维护 $l = r$ $mod$  $k$ ，即当前位置的**余数**。
 
---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func maxSubarraySum(nums []int, k int) int64 {
	minS := make([]int, k)
	for i := range k - 1 {
		minS[i] = math.MaxInt / 2
	}

	ans := math.MinInt
	s := 0
	for j, x := range nums {
		s += x
		i := j % k
		ans = max(ans, s-minS[i])
		minS[i] = min(minS[i], s)
	}
	return int64(ans)
}
```
---

## C++ 代码

```C++
class Solution {
public:
    long long maxSubarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        vector<long long> sum(n+1);
        vector<long long> mins(k, LLONG_MAX/2);
        long long ans = LLONG_MIN;
        for (int i = 0; i < n; i++) {
            sum[i+1] = sum[i] + nums[i];
        }
        for (int i = 0; i <= n; i++) {
            int j = i % k;
            ans = max(ans, sum[i] - mins[j]);
            mins[j] = min(mins[j], sum[i]);
        }
        return ans;
    }
};
```
---
## Python 代码

```Python
class Solution:
    def maxSubarraySum(self, nums: List[int], k: int) -> int:
        n = len(nums)
        s = [0] * (n+1)
        for i, x in enumerate(nums):
            s[i+1] = s[i] + x
        mins = [inf] * (n+1)
        ans = -inf
        for i, s in enumerate(s):
            j = i % k
            ans = max(ans, s-mins[j])
            mins[j] = min(mins[j], s)
        return ans
```

---

## JavaScript 代码

```JavaScript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var maxSubarraySum = function(nums, k) {
    const n = nums.length;
    let ans = -Infinity;
    const sum = Array(n+1).fill(0);
    for (let i = 0; i < n; i++) {
        sum[i+1] = sum[i] + nums[i]
    }
    const min = Array(k).fill(Infinity);
    for (let i = 0; i <= n; i++) {
        let j = i % k;
        ans = Math.max(ans, sum[i] - min[j]);
        min[j] = Math.min(min[j], sum[i])
    }
    return ans;
};
```
