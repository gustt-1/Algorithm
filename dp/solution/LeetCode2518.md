# [LeetCode-2518] [好分区的数目](https://leetcode.cn/problems/number-of-great-partitions/)
## 题目描述 

> 给你一个正整数数组 nums 和一个整数 k 。分区 的定义是：将数组划分成两个有序的 组 ，并满足每个元素 恰好 存在于 某一个 组中。如果分区中每个组的元素和都大于等于 k ，则认为分区是一个好分区。返回 不同 的好分区的数目。由于答案可能很大，请返回对 109 + 7 取余 后的结果。
如果在两个分区中，存在某个元素 nums[i] 被分在不同的组中，则认为这两个分区不同。

---

## 我的思路
**0-1背包**

##

选出好分区，也就是从数组中选出两堆数组成新的两个数组，并且这两个数组分别的总和都要大于等于k，我们容易联想到0-1背包。但是好分区背包容量过大容易超时，我们可以思考反着做。也就是找出坏分区的数目（坏分区是只要一个分组小于k就是坏分区）。对于整个分区集合，假设我们有集合ABCD，并且每个集合都分成数组1和数组2：集合A为数组1,2总和都大于等于k（好分区）；集合B为数组1大于k且数组2小于k（坏分区）；集合C为数组1小于k且数组2大于等于k（坏分区），集合D为数组1,2都小于等于k（坏分区）。可以得出分区全集中，除了好分区就是坏分区，并且坏分区具有对称性。因为对于两个数组1,2将1和2的所有数字交换后又会得到一个新的分区。

那么我们的目标就明确了，就是找到坏分区的数目，再用全部分区的数目减去两倍的坏分区的数目（具有对称性）

- 1.我们的背包总容量为k，由于下标从零开始所以最多到f[k - 1]，也就是小于k（坏分区）。
- 2.进入循环，第一层为枚举全部数，第二层则是枚举背包容量，同样我们可以用到0-1背包空间压缩的技巧。由于答案需要取模，状态转移中我们每一步都需要进行模运算：f[j] = (f[j] + f[j - x]) % mod
- 3.最后我们枚举f数组，加上所有的不满足总和小于k的坏分区数
- 4.对于总分区数，也就是从n个数中分成两堆,对于每个数要么就是在第1堆，要么在第2堆，就是两种情况。所以总分区数目有2ⁿ个，我们就在枚举全部数时也顺便计算2ⁿ。也就是res = res * 2 % mod，记得取模运算。最后枚举f数组时再减去坏分区数res即是答案了：res -= n * 2，由于是减法运算所以最后模运算是(res % mod + mod) % mod



##
---

## 时间复杂度

O(nk)，其中 n 为 nums 的长度。

---

## 空间复杂度

O(k)

---

## Go 代码

```Go

func countPartitions(nums []int, k int) int {
    mod := int(1e9 + 7)
    sum := 0
    for _, x := range nums {
        sum += x
    }
    if sum < 2 * k {
        return 0
    }
    res := 1
    f := make([]int,k)
    f[0] = 1
    for _, x := range nums {
        res = res * 2 % mod
        for j := k - 1; j >= x; j-- {
            f[j] = (f[j] + f[j - x]) % mod
        }
    }
    for _, n := range f {
		res -= n * 2
	}
	return (res % mod + mod) % mod
}

```
---

## C++ 代码

```C++

class Solution {
public:
    int countPartitions(vector<int>& nums, int k) {
        const int mod = 1e9 + 7;
        if (accumulate(nums.begin(), nums.end(), 0L) < k * 2) return 0;
        vector<int> f(k);
        f[0] = 1;
        int res = 1;
        for (int x : nums) {
            res = res * 2 % mod;
            for (int j = k - 1; j >= x; j--) {
                f[j] = (f[j] + f[j - x]) % mod;
            }
        }
        for (int n : f) {
            res = (res - 2 * n % mod + mod) % mod;
        }
        return res;
    }
};


```
---
## Python 代码

```Python

class Solution:
    def countPartitions(self, nums: List[int], k: int) -> int:
        mod = 10 ** 9 + 7
        total = sum(nums)
        if total < k * 2 : return 0
        f = [0] * k
        f[0] = 1
        res = 1
        for x in nums :
            res = (res * 2) % mod
            for j in range(k - 1, x - 1,-1) :
                f[j] = (f[j] + f[j - x]) % mod
        for x in f :
            res = (res - x * 2 + mod) % mod
        return res

```
