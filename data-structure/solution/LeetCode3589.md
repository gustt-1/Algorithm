# [LeetCode3589] [计数质数间隔平衡子数组](https://leetcode.cn/problems/count-prime-gap-balanced-subarrays/description/)

## 题目描述
给定一个整数数组 `nums` 和一个整数 `k` 。

子数组 被称为 **质数间隔平衡**，如果：

其包含 **至少两个质数**，并且
该 **子数组** 中 **最大** 和 **最小** 质数的差小于或等于 `k`。
返回 `nums` 中质数间隔平衡子数组的数量。

注意：

子数组 **是数组中连续的** **非空** 元素序列。
质数是大于 $1$ 的自然数，它只有两个因数，即 $1$ 和**它本身**。

---

## 题目大意



---

## 输入




## 输出

> 

---

## 我的思路

**单调队列+滑动窗口**

> 窗口（子数组）越长，所包含的质数的最大值越大，最小值越小，质数极差越大；反之，窗口越短，质数极差越小。我们可以用滑动窗口来解决。同时我们维护**两个单调队列**，一个单调递增队列，一个单调递减队列，分别维护窗口内的素数**最大值**和**最小值**。

> 我们先要判断当前是是否为素数，可以先用**埃氏筛**来预处理出所有的素数。

> 同时维护两个素数下标，一个为倒数第二个($last2$)  ，一个为最后一个($last1$) 。我们**枚举每个右端点**，满足要求时，当前窗口（子数组）对答案的贡献即为 $last2 - l + 1$ 即符合要求的**左端点的个数**。


---

## 时间复杂度

$O(n)$ 

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
const mx = 5000001
var isprime [mx]bool
var primes []int

func sieve() {
	for i := 2; i < mx; i++ {
		isprime[i] = true
	}

	for i := 2; i < mx; i++ {
		if isprime[i] {
			primes = append(primes, i)
		}
		for j := i * i; j < mx; j += i {
			isprime[j] = false
		}
	}
}

func init() {
	sieve()
}

func primeSubarray(nums []int, k int) (ans int) {
    last1, last2 := -1, -1
    var maxv, minv []int
    l := 0
    for i, x := range nums {
        if isprime[x] {
            last2 = last1
            last1 = i
            for len(maxv) > 0 && x >= nums[maxv[len(maxv)-1]] {
                maxv = maxv[:len(maxv)-1]
            }
            maxv = append(maxv, i)

            for len(minv) > 0 && x <= nums[minv[len(minv)-1]] {
                minv = minv[:len(minv)-1]
            }
            minv = append(minv, i)


            for nums[maxv[0]] - nums[minv[0]] > k {
                l++
                if minv[0] < l {
                    minv = minv[1:]
                }
                if maxv[0] < l {
                    maxv = maxv[1:]
                }
            }
        }
        ans += last2 - l + 1
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

```

---

## JavaScript 代码

```JavaScript
```
