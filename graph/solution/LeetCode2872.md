# [2872] [可以被 K 整除连通块的最大数目](https://leetcode.cn/problems/maximum-number-of-k-divisible-components/description/)

## 题目描述
给你一棵 `n` 个节点的无向树，节点编号为 `0` 到 `n - 1` 。给你整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 $edges[i] = [a_i, b_i]$ 表示树中节点 $a_i$ 和 $b_i$ 有一条边。

同时给你一个下标从 0 开始长度为 n 的整数数组 $values$ ，其中 $values[i]$ 是第 `i` 个节点的 值 。再给你一个整数 `k` 。

你可以从树中删除一些边，也可以一条边也不删，得到若干连通块。一个 连通块的值 定义为连通块中所有节点值之和。如果所有连通块的值都可以被 $k$ `整除`，那么我们说这是一个 `合法分割` 。

请你返回所有`合法分割`中，连通块数目的`最大值` 。

---

## 题目大意


---

## 输入

> 


## 输出

> 

---

## 我的思路

**DFS**

> 如果一个两个连通块的都能被 $k$ 整除，那么这两个连通块的和也能被 $k$ 整除。反过来说如果 $x+y$ 不是 $k$ 的倍数，那么 $x$ 和 $y$ `不全是` $k$ 的倍数。不是 $k$ 的倍数的数，继续拆分，`始终存在一个`不是 $k$ 的倍数的数。

> 对应到删边上，删除一条边后，我们把一个连通块分成了两个连通块。如果其中一个连通块的点权和不是 $k$ 的倍数，那么这个连通块无论如何分割，始终存在一个点权和不是 $k$ 的倍数的连通块。所以当且仅当这两个连通块的点权和都是 $k$ 的倍数，这条边才能删除。

> 删除后，由于分割出的连通块点权和仍然是 $k$ 的倍数，所以可以继续分割，直到无法分割为止。换句话说，只要有能删除的边，就删除。

## 作者：[灵茶山艾府](https://leetcode.cn/problems/maximum-number-of-k-divisible-components/solutions/2464687/pan-duan-zi-shu-dian-quan-he-shi-fou-wei-uvsg/)


 
---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func maxKDivisibleComponents(n int, edges [][]int, values []int, k int) (ans int) {
    g := make([][]int, n)
    for _, e := range edges {
        x, y := e[0], e[1]
        g[x] = append(g[x], y)
        g[y] = append(g[y], x)
    }

    var dfs func(int, int) int
    dfs = func(x, fa int) int {
        v := values[x]
        for _, y := range g[x] {
            if y != fa {
                v += dfs(y, x)
            }
        }
        if v % k == 0 {
            ans++
        }
        return v
    }
    dfs(0, -1)
    return
}
```
---

## C++ 代码

```C++
class Solution {
public:
    int maxKDivisibleComponents(int n, vector<vector<int>>& edges, vector<int>& values, int k) {
        vector<vector<int>> g(n);
        for (auto e : edges) {
            int x = e[0], y = e[1];
            g[x].push_back(y);
            g[y].push_back(x);
        }
        int ans = 0;
        auto dfs = [&](this auto &dfs, int x, int fa) -> long long {
            long long v = values[x];
            for (auto y : g[x]) {
                if (y != fa) {
                    v += dfs(y, x);
                }
            }
            if (v % k == 0) {
                ans++;
            }
            return v;
        };
        dfs(0, -1);
        return ans;
    }
};
```
---
## Python 代码

```Python
class Solution:
    def maxKDivisibleComponents(self, n: int, edges: List[List[int]], values: List[int], k: int) -> int:
        g = [[] for _ in range(n)]
        for x, y in edges:
            g[x].append(y)
            g[y].append(x)
        ans = 0
        def dfs(x: int, fa: int) -> int:
            v = values[x]
            for y in g[x]:
                if y != fa:
                    v += dfs(y, x)
            nonlocal ans
            if v % k == 0:
                ans += 1
            return v
        
        dfs(0, -1)
        return ans
```

---

## JavaScript 代码

```JavaScript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @param {number[]} values
 * @param {number} k
 * @return {number}
 */
var maxKDivisibleComponents = function(n, edges, values, k) {
    const g = Array.from({ length: n }, () => []);
    for (const [x, y] of edges) {
        g[x].push(y);
        g[y].push(x);
    }

    let ans = 0;

    function dfs(x, fa) {
        let sum = values[x];
        for (const y of g[x]) {
            if (y !== fa) {
                sum += dfs(y, x);
            }
        }
        ans += sum % k === 0 ? 1 : 0;
        return sum;
    }

    dfs(0, -1);
    return ans;
};
```
