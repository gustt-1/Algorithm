# [leetcode-924] 尽量减少恶意软件的传播

## 题目描述


> 给出了一个由 n 个节点组成的网络，用 n × n 个邻接矩阵图 graph 表示。在节点网络中，当 graph[i][j] = 1 时，表示节点 i 能够直接连接到另一个节点 j。一些节点 initial 最初被恶意软件感染。只要两个节点直接连接，且其中至少一个节点受到恶意软件的感染，那么两个节点都将被恶意软件感染。这种恶意软件的传播将继续，直到没有更多的节点可以被这种方式感染。假设 M(initial) 是在恶意软件停止传播之后，整个网络中感染恶意软件的最终节点数。如果从 initial 中移除某一节点能够最小化 M(initial)， 返回该节点。如果有多个节点满足条件，就返回索引最小的节点。请注意，如果某个节点已从受感染节点的列表 initial 中删除，它以后仍有可能因恶意软件传播而受到感染。

---

## 我的思路
**本题使用深度优先搜索（DFS）**


![alt text](image.png)
## 

我们先根据graph建立无向图，如果一个节点是感染的，那么与他相连或者间接相连的都会被感染（如图，红色的是被感染的）。所以当一个连通分量中有两个及以上的节点感染，那么这个连通分量无论删除哪个节点的病毒都不能阻止一部分受到感染（注意是删除节点的病毒而不是删除节点）。要使得最终受到敢感染的节点数尽量小，那么我们的问题便转化成了求一个只感染了一个病毒的最大的连通分量，这样只要删除了最大的连通分量中的哪个节点的病毒，那么整个连通分量就都不会被感染了。

1.我们先创建一个长度为节点数量的数组isinitial，用于记录哪个节点是被感染的（被感染则为true,反之为false）以及一个vis数组用于记录当前要访问的节点是否已经被访问。

2.再使用size,node用于记录连通分量的大小，以及当前连通分量的删除病毒的状况。初始化node = -1（当前连通分量没有病毒），要是找到了一个病毒那么node = x（x为当前期望删除病毒的节点），如果找到两个及以上那么我们让node = -2（因为我们期望删除病毒的节点都是整数，那么我们就让node = -2 来避免与节点下标产生冲突）

3.接下来就是dfs遍历，首先标记vis[x] = ture。然后如果node != -2 并且当前节点是病毒 并且node < 0 那么就让node = x（变成期望删除节点的下标），如果node >= 0（意味着之前就已经找到了一个病毒node是那个病毒节点）那么我们就让node = -2。接着就是遍历其他节点

4.我们再变量是病毒的节点，让size = 0，node = -1。如果node >= 0表示找到了一个只有一个病毒节点的连通分量，node就是那个节点下标。那么我们再去比较size和maxSize，如果size大于maxSize或者size等于maxSize并且node < res 那么我们记录答案 res = node并且使maxSize = size。最后返回res即可

##

---

## 时间复杂度

O( n² )，其中 n 为 graph 的长度

---

## 空间复杂度

O(n)

---

## Go 代码

```Go

func minMalwareSpread(graph [][]int, initial []int) int {
    vis := make([]bool,len(graph))
    isinitial := make([]bool,len(graph))
    for _, x := range initial {
        isinitial[x] = true
    }
    var size, node int 
    var dfs func(int)
    dfs = func(x int) {
        vis[x] = true
        size++
        if node != -2 && isinitial[x] {
            if node < 0 {     // 没有感染的
                node = x    // 感染节点标记为节点索引
            } else {
                node = -2        // 一个连通块中有两个及以上感染，node标记为-2
            }
        }
        for y, conn := range graph[x] {
            if conn == 1 && !vis[y] {
                dfs(y)
            }
        }
    }
    res := -1
    maxSize := 0
    for _, x := range initial {
        if vis[x] {
            continue
        }
        node = -1
        size = 0
        dfs(x)
        if node >= 0 && (size > maxSize || size == maxSize && node < res) {
            res = node
            maxSize = size
        }
    }
    if res < 0 {
        return slices.Min(initial)
    }
    return res
}

```
---

## C++ 代码

```C++

class Solution {
public:
    int minMalwareSpread(vector<vector<int>>& graph, vector<int>& initial) {
        int n = graph.size();
        vector<bool> isinitial(n);
        vector<bool> vis(n);
        int size, node;
        for (int x : initial) {
            isinitial[x] = true;
        }
        auto dfs = [&](this auto&&dfs,int x) -> void {
            vis[x] = true;
            size++;
            if (node != -2 && isinitial[x]) {
                if (node < 0) {      // 没有感染的
                    node = x;       // 感染节点标记为节点索引
                } else {
                    node = -2;     // 一个连通分量中有两个及以上感染，node标记为-2
                }
            }
            for (int y = 0; y < graph[x].size(); y++) {
                if (graph[x][y] == 1 && !vis[y]) {
                    dfs(y);
                }
            }
        };
        int res = -1, maxSize = 0;
        for (int x : initial) {
            if (vis[x]) {
                continue;
            }
            node = -1;
            size = 0;
            dfs(x);
            if (node >= 0 && (size > maxSize || size == maxSize && node < res)) {
                res = node;
                maxSize = size;
            }
        }
        return res < 0 ? *min_element(initial.begin(), initial.end()) : res;
    }
};

```
---
## Python 代码

```Python

class Solution:
    def minMalwareSpread(self, graph: List[List[int]], initial: List[int]) -> int:
        n = len(graph)
        isinitial = [False] * n
        for x in initial:
            isinitial[x] = True
        
        vis = [False] * n
        res = -1
        maxSize = 0

        def dfs(x):
            nonlocal size, node
            vis[x] = True
            size += 1
            if node != -2 and isinitial[x]:
                if node < 0:
                    node = x
                else:
                    node = -2
            for y in range(n):
                if graph[x][y] == 1 and not vis[y]:
                    dfs(y)

        for x in initial:
            if vis[x]:
                continue
            size = 0
            node = -1
            dfs(x)
            if node >= 0 and (size > maxSize or (size == maxSize and node < res)):
                res = node
                maxSize = size

        return min(initial) if res < 0 else res

```
