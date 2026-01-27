# 算法竞赛模板

```
package main

import (
	"bufio"
	. "fmt"
	"io"
	"os"
)

const inf = 0x3f3f3f3f

type Number interface {
	~int | ~int8 | ~int32 | ~int64 |
		~uint | ~uint32 | ~uint64 |
		~float32 | ~float64
}

type Signed interface {
	~int | ~int8 | ~int32 | ~int64
}

type (
	i8  = int8
	u64 = uint64
	u32 = uint32
	f32 = float32
	f64 = float64
)

func abs[T Signed](x T) T {
	if x < 0 {
		return -x
	}
	return x
}

func gcd[T Signed](a, b T) T {
	for a != 0 {
		a, b = b%a, a
	}
	return b
}

func lcm[T Signed](a, b T) T {
	if a == 0 || b == 0 {
		return 0
	}
	return a / gcd(a, b) * b
}

func max[T Number](x, y T) T {
	if x > y {
		return x
	}
	return y
}

func min[T Number](x, y T) T {
	if x < y {
		return x
	}
	return y
}

func solve(in io.Reader, out io.Writer) {
	var T int
	for Fscan(in, &T); T > 0; T-- {

	}
}

func main() {
	solve(bufio.NewReader(os.Stdin), os.Stdout)
}

```

---


## 数据结构

### [堆](Go/Heap.md)
### [树状数组](Go/Fenwick_Tree.md)
### [前缀树](Go/Trie.md)

---

## 图论


---
## 动态规划
### [矩阵快速幂](Go/Matrix_Fast_Power.md)
### [数位DP](Go/Digit_Dynamic_Programming.md)



---

## 数学

### [乘法快速幂](Go/Fast_Power.md)
### [质数筛（埃氏筛/欧拉筛）](Go/isprime.md)
### [gcd/lcm](Go/gcd_and_lcm.md)
### [二进制回文数预处理](Go/erjinzhihuiwen.md)


---

## 字符串

### [子序列自动机](Go/Subsequence_Automaton.md)
### [kmp](Go/kpm.md)
---
