# 状态转移方程 $f[n] = c_1 * f[n-1] + c_2 * f[n-2] + ... + c_k * f[n-k]$

---

构造一列长度为 `k` 的单列矩阵 `f0` ，`f0[0][0], f0[1][0], f0[2][0], ..., f[k-1][0]`

**状态向量** `f0` 就是递推所需的前 $k$ 个初始值

---
一个快速幂矩阵 `T` , `T` 为 `k*k` 的矩阵，其中 $T[0][i] = c_i$ (第一行即是状态转移方程的各个**系数常量**)

并且 `for i := 0; i < k - 1; i++ { T[i+1][i] = 1 }` 其他全为**零**

答案即是`fn[0][0]`

---

## 如果是**二维**且不是一样的多项，例如转移方程如下：
`0: 'a', 1: 'e', 2: 'i', 3: 'o', 4: 'u'`

`f[0][i+1] = f[1][i] + f[2][i] + f[4][i] `

`f[1][i+1] = f[0][i] + f[2][i]`

`f[2][i+1] = f[1][i] + f[3][i] `

`f[3][i+1] = f[2][i] `

`f[4][i+1] = f[2][i] + f[3][i]`

我们每行就是对应的 `1, 2, 3, 4, 5` 我们是否需要加 $f[k][i]   (k∈[0,4])$ ，我们可以参考**行列式计算**。

`k 行 1 列` **状态向量**对应 `1, 2, 3, 4, 5`

比如 `f[0][i+1] = f[1][i] + f[2][i] + f[4][i]` ，那么就在 $T$ 矩阵中 `T[0][1] = 1, T[0][2] = 1, T[0][3] = 0, T[0][4] = 1`

每种 $i$ 答案即是 `fn[i][0]` 。


---
## 如下图：

`f0矩阵`：

     | f[k-1] |  <- 最新值 f[n-1]
     | f[k-2] |
     | ...    |
     | f[0]   |  <- 最旧值 f[n-k]

---

`T矩阵`：

    | c1   c2   c3  ...    ck |
    | 1    0    0   ...    0  |
    | 0    1    0   ...    0  |
    | 0    0    1   ...    0  |
    | ...               ...   |
    | 0    0    0  ...  1  0  |

---
---
## 模板代码（若要取模就改一下 $mod$ ），不要就删掉运算中的取模即可：
``` Go
const mod = int(1e9 + 7)

type matrix [][]int

func newMatrix(n, m int) matrix {
	a := make(matrix, n)
	for i := range a {
		a[i] = make([]int, m)
	}
	return a
}

func (a matrix) mul(b matrix) matrix {
	c := newMatrix(len(a), len(b[0]))
	for i, row := range a {
		for k, x := range row {
			if x == 0 {
				continue
			}
			for j, y := range b[k] {
				c[i][j] = (c[i][j] + x*y) % mod
			}
		}
	}
	return c
}

func (a matrix) powMul(n int, f0 matrix) matrix {
	res := f0
	for ; n > 0; n /= 2 {
		if n&1 != 0 {
			res = a.mul(res)
		}
		a = a.mul(a)
	}
	return res
}
```
---

