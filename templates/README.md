# 算法竞赛模板

```
package main

import (
	"bufio"
	. "fmt"
	"io"
	"os"
)

type (
	i64  = int64
	i32  = int32
	ui64 = uint64
	ui32 = uint32
)

func solve(in io.Reader, out io.Writer) {
	var T int
	for Fscan(in, &T); T > 0; T-- {
		
	}
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

func main() {
	solve(bufio.NewReader(os.Stdin), os.Stdout)
}

```

---


## 数据结构

### [堆](Go/Heap.md)
### [树状数组](Go/Fenwick_Tree.md)


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
