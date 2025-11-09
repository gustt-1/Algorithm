# [LeetCode1611] [使整数变为 0 的最少操作次数](https://leetcode.cn/problems/minimum-one-bit-operations-to-make-integers-zero/description/)

## 题目描述
给你一个整数 `n` ，你需要重复执行多次下述操作将其转换为 `0` ：

翻转 `n` 的二进制表示中最右侧位（第 `0` 位）。
如果第 `(i-1)` 位为 `1` 且从第 `(i-2)` 位到第 `0` 位都为 `0`，则翻转 `n` 的二进制表示中的第 `i` 位。
返回将 `n` 转换为 `0` 的最小操作次数。

---


## 输入

>


## 输出

>

---

## 我的思路
**数学**

> 计算 `10` 需要多少次。 $10 -> 11 -> 01 -> 00$ , 需要 	`3` 次

> 计算 `100` , $100 -> 101 -> 111 -> 110 -> 010 -> 011 -> 001 -> 000$ , 需要 `7` 次

> 观察可以看到 `100` 变成 `0` 需要 $f(10) + 1 + f(10) = 7$ 次。 $$100 -> 101 -> 111[f(10)] -> 110(+1) -> 010 -> 011 -> 001 -> 000[f(10)]$$ 

> 同理可得到 $f(1000) = f(100) + 1 + f(100) = 15$ 。


> 那么一般地，我们有 $$f(2^k) = 2f(2^{k-1}) + 1$$
即 $$f(2^k) + 1 = 2f(2^{k-1} + 1)$$ 。所以 $$f(2^k)+1 是个等比数列，公比为  2，初始值为 f(1)+1=2 $$ ，得 $$f(2^k) = 2^{k+1} - 1$$

**上面为 $n$ 为 $2$ 的幂的情况**

> 一般地，设 $n$ 的二进制长度为 $k$ ，我们有 $$f(n) = f(2^{k-1}) - f(n-2^{k-1})= 2^k - 1 - f(n-2^{k-1})$$ 。其中 $n−2^{k−1}$ 表示 $n$ 去掉最高的 $1$ 后的值。递归边界： $f(0) = 0$ 。

---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func minimumOneBitOperations(n int) int {
    if n == 0 {
		return 0
	}
	k := bits.Len(uint(n))
	return (1<<k) - 1 - minimumOneBitOperations(n-1<<(k-1))
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
    def minimumOneBitOperations(self, n: int) -> int:
        if n == 0:
            return 0
        k = n.bit_length()
        return (1 << k) - 1 - self.minimumOneBitOperations(n - (1 << (k - 1)))
```
