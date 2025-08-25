# [leetcode-930] [和相同的二元子数组](https://leetcode.cn/problems/binary-subarrays-with-sum/description/)
## 题目描述 

> 给你一个二元数组 nums ，和一个整数 goal ，请你统计并返回有多少个和为 goal 的 非空 子数组。子数组
是数组的一段连续部分。

> 示例 1：输入：nums = [1,0,1,0,1], goal = 2   输出：4   解释：有 4 个满足题目要求的子数组：[1,0,1]、[1,0,1,0]、[0,1,0,1]、[1,0,1]



---

## 我的思路
**前缀和+哈希表/ 滑动窗口**

##

> 前缀和+哈希表
  - 求和我们可以使用枚举当前下标r的前缀和，维护一个下标l的前缀和的哈希表的个数
  - 例如，当我们枚举到r时，我们当前（1-r）的和为sum，那么我们就去哈希表里面找sum-goal是否有值，如果有我们就将答案加上哈希表的值，也就是个数；如果没有那么我们就将sum加入到哈希表中cnt[sum]++。可另见前缀和+哈希表题解（[点击跳转]()）


> 滑动窗口
  - 我们使用不定长滑动窗口，我们使用一个函数来找不大于goal的子数组的个数和不大于goal-1的子数组的个数。那么我们两者相减就是恰好为gaol的子数组的个数
  - 那么我们从0开始累加元素，只要我们当前累加和小于我们要找到目标值，那么我们就加入答案（r-l+1）。大于的话我们就用一个循环来缩小左窗口，直到累加和小于目标值。

##
---

## 时间复杂度



---

## 空间复杂度



---

## Go 代码

```Go

func numSubarraysWithSum(nums []int, goal int) (ans int) {
    sum := 0
    cnt := map[int]int{0: 1}
    for _, x := range nums {
        sum += x
        if c, ok := cnt[sum - goal]; ok {
            ans += c
        }
        cnt[sum]++
    }
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
    def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
        total = 0
        cnt = defaultdict(int)
        cnt[0] = 1
        ans = 0
        for _, x in enumerate(nums):
            total += x
            if total-goal in cnt:
                ans += cnt[total-goal]
            cnt[total] += 1
        return ans



```
