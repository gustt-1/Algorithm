# [leetcode-2192] 有向无环图中一个节点的所有祖先

## 题目描述


> 给你一个正整数 n ，它表示一个 有向无环图 中节点的数目，节点编号为 0 到 n - 1 （包括两者）。给你一个二维整数数组 edges ，其中 edges[i] = [fromi, toi] 表示图中一条从 fromi 到 toi 的单向边。请你返回一个数组 answer，其中 answer[i]是第 i 个节点的所有 祖先 ，这些祖先节点 升序 排序。如果 u 通过一系列边，能够到达 v ，那么我们称节点 u 是节点 v 的 祖先 节点。

---

## 我的思路
**本体使用深度优先搜索（DFS）**

由一个结点指向另一个结点（0 -> 1）那么1就是0的一个祖先，那么我们就可以从i结点出发，能访问到的结点就是它的祖先，就加入答案中。我们可以用dfs来搜索，dfs过程中用vis数组来标记访问过的点，在从一个节点i出发dfs后我们标记的节点就是i的祖先，但是注意我们一进入dfs也会标记节点i本身，所以我们需要把i去掉即可。并且每一次dfs后需要清空vis数组，因为对于其他的j,已经访问过的节点i的祖先也可能是j的祖先。

---

## 时间复杂度

O(n(n+m))。其中 m 是 edges 的长度。对于每个起点 i，跑一次 DFS 的时间复杂度为 O(n+m)。

---

## 空间复杂度

O(n+m)

---

## Go 代码

```Go

func getAncestors(n int, edges [][]int) [][]int {
    g := make([][]int, n)
    for _, e := range edges {
        x, y := e[0], e[1]
        g[y] = append(g[y], x)
    }

    vis := make([]bool, n)
    var dfs func(int)
    dfs = func(x int) {
        vis[x] = true
        for _, y := range g[x] {
            if !vis[y] {
                dfs(y)
            }
        }
    }
    ans := make([][]int, n)
    for i := range ans {
        clear(vis)
        dfs(i)
        vis[i] = false
        for j, b := range vis {
            if b {
                ans[i] = append(ans[i], j)
            }
        }
    }
    return ans
}

```
---

## C++ 代码

```C++

class Solution {
public:
    vector<vector<int>> getAncestors(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int x = e[0], y = e[1];
            g[y].push_back(x);
        }

        vector<vector<int>> ans(n);
        vector<bool> vis(n);
        
        auto dfs = [&](this auto&&dfs,int x) -> void {
            vis[x] = true;
            for (int y : g[x]) {
                if (!vis[y]) {
                    dfs(y);
                }
            }
        };

        for (int i = 0; i < n; ++i) {
            fill(vis.begin(), vis.end(), false);
            dfs(i);
            vis[i] = false;
            for (int j = 0; j < n; ++j) {
                if (vis[j]) {
                    ans[i].push_back(j);
                }
            }
        }

        return ans;
    }
};

```
---
## Python 代码

```Python

class Solution:
    def getAncestors(self, n: int, edges: List[List[int]]) -> List[List[int]]:
        g = [[] for _ in range(n)]
        for x, y in edges:
            g[y].append(x)

        ans = [[] for _ in range(n)]
        vis = [False] * n

        def dfs(x):
            vis[x] = True
            for y in g[x]:
                if not vis[y]:
                    dfs(y)

        for i in range(n):
            vis = [False] * n
            dfs(i)
            vis[i] = False
            for j in range(n):
                if vis[j]:
                    ans[i].append(j)
        return ans

```
