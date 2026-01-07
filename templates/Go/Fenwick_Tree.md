

## Go

``` Go
// 模板来源 https://leetcode.cn/circle/discuss/mOr1u6/
type fenwick []int

func newFenwickTree(n int) fenwick {
    return make(fenwick, n+1) // 使用下标 1 到 n
}

// a[i] 增加 val
// 1 <= i <= n
// 时间复杂度 O(log n)
func (f fenwick) update(i int, val int) {
    for ; i < len(f); i += i & -i {
        f[i] += val
    }
}

// 求前缀和 a[1] + ... + a[i]
// 1 <= i <= n
// 时间复杂度 O(log n)
func (f fenwick) pre(i int) (res int) {
    for ; i > 0; i &= i - 1 {
        res += f[i]
    }
    return
}

// 求区间和 a[l] + ... + a[r]
// 1 <= l <= r <= n
// 时间复杂度 O(log n)
func (f fenwick) query(l, r int) int {
    if r < l {
        return 0
    }
    return f.pre(r) - f.pre(l-1)
}

// by灵茶山艾府
// https://leetcode.cn/discuss/post/mOr1u6/
```


## Python
``` Python
# 模板来源 https://leetcode.cn/circle/discuss/mOr1u6/
class FenwickTree:
    def __init__(self, n: int):
        self.tree = [0] * (n + 1)  # 使用下标 1 到 n

    # a[i] 增加 val
    # 1 <= i <= n
    # 时间复杂度 O(log n)
    def update(self, i: int, val: int) -> None:
        t = self.tree
        while i < len(t):
            t[i] += val
            i += i & -i

    # 计算前缀和 a[1] + ... + a[i]
    # 1 <= i <= n
    # 时间复杂度 O(log n)
    def pre(self, i: int) -> int:
        t = self.tree
        res = 0
        while i > 0:
            res += t[i]
            i &= i - 1
        return res

    # 计算区间和 a[l] + ... + a[r]
    # 1 <= l <= r <= n
    # 时间复杂度 O(log n)
    def query(self, l: int, r: int) -> int:
        if r < l:
            return 0
        return self.pre(r) - self.pre(l - 1)
# by灵茶山艾府
# https://leetcode.cn/discuss/post/mOr1u6/
```
