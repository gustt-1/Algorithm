# [leetcode-699] [掉落的方块](https://leetcode.cn/problems/falling-squares/description/)
## 题目描述 

> 在二维平面上的 x 轴上，放置着一些方块。给你一个二维整数数组 positions ，其中 positions[i] = [lefti, sideLengthi] 表示：第 i 个方块边长为 sideLengthi ，其左侧边与 x 轴上坐标点 lefti 对齐。每个方块都从一个比目前所有的落地方块更高的高度掉落而下。方块沿 y 轴负方向下落，直到着陆到 另一个正方形的顶边 或者是 x 轴上 。一个方块仅仅是擦过另一个方块的左侧边或右侧边不算着陆。一旦着陆，它就会固定在原地，无法移动。在每个方块掉落后，你必须记录目前所有已经落稳的 方块堆叠的最高高度 。返回一个整数数组 ans ，其中 ans[i] 表示在第 i 块方块掉落后堆叠的最高高度

---

## 我的思路
**动态开点线段树**

##

- 1. 根据题意，我们需要查询一个区间的最大高度，容易联想到使用线段树。一个方块的下落也就对于一个区间进行了修改。那么我们用change表示修改的值，update用于标记是否含有懒更新信息，maxh则表示一个区间的最大高度（用于查询）。但是根据题目数据范围1 <= lefti <= 10的8次方。并且1 <= sideLengthi <= 10的6次方。也就是说一个方块左边界是10的8次方，长度是10的6次方，那么数据范围是非常大的，如果以方块的长度作为数组，最大还需要开4n的空间，是很大的。那么我们可以用离散化来构建线段树。


- 2. 开一个数组arr用于存放每个方块的左边界和右边界，考虑方块能擦边下落（[0,1] [1,2]）可以落下两个方块，那么我们就用左闭右开区间来记录例如[2,3]我们就记录成[2,2]。 arr数组的长度我们只需要开到len(positions)*2+1即可，大大节省了空间。我们需要获取查询数组的左右边界时只需要用一个二分查找即可。

- 3. 后面便是build建树，up为汇总区间的最大值，lazy为停留住updateFunc的change信息，change为需要修改成的值（方块高度）。



##
---

## 时间复杂度

每次查询和修改的时间复杂度为 O(logn)，总时间复杂度为 O(nlogn)

---

## 空间复杂度

O(2*n) n = positions.length

---

## Go 代码

```Go

func fallingSquares(positions [][]int) []int {
    const MAXN = 2001
    var (
        arr = make([]int, MAXN)
        maxh = make([]int, MAXN << 2)
        update = make([]bool, MAXN << 2)
        change = make([]int, MAXN << 2)
    )

    collect := func(pos [][]int) int {
        cnt := 1
        for _, p := range pos {
            arr[cnt] = p[0]
            cnt++
            arr[cnt] = p[0] + p[1] - 1
            cnt++
        }
        sort.Ints(arr[1:cnt])
        n := 1
        for i := 2; i < cnt; i++ {
            if arr[n] != arr[i] {
                n++
                arr[n] = arr[i]
            }
        }
        return n
    }

    rank := func(n, v int) int {
        ans := 0
        l, r := 1, n
        for l <= r {
            mid := (l + r) >> 1
            if arr[mid] >= v {
                ans = mid
                r = mid - 1
            } else {
                l = mid + 1
            }
        }
        return ans
    }

    up := func(i int) {
        maxh[i] = max(maxh[i << 1],maxh[i << 1 | 1])
    }

    lazy := func(i, v int) {
        update[i] = true
        change[i] = v
        maxh[i] = v
    }

    var down func(i int)
    down = func(i int) {
        if update[i] {
            lazy(i << 1, change[i])
            lazy(i << 1 | 1, change[i])
            update[i] = false
        }
    }

    var build func(l, r, i int)
    build = func(l, r, i int) {
        if l < r {
            mid := (l + r) >> 1
            build(l, mid, i << 1)
            build(mid + 1, r, i << 1 | 1)
        }
        maxh[i] = 0
        update[i] = false
        change[i] = 0
    }

    var updateFunc func(jobl, jobr, jobv, l, r, i int)
    updateFunc = func(jobl, jobr, jobv, l, r, i int) {
        if jobl <= l && r <= jobr {
            lazy(i, jobv)
        } else {
            mid := (l + r) >> 1
            down(i)
            if jobl <= mid {
                updateFunc(jobl, jobr, jobv, l, mid, i << 1)
            }
            if jobr > mid {
                updateFunc(jobl, jobr, jobv, mid + 1, r, i << 1 | 1)
            }
            up(i)
        }
    }

    var query func(jobl, jobr, l, r, i int) int
    query = func(jobl, jobr, l, r, i int) int {
        if jobl <= l && r <= jobr {
            return maxh[i]
        }
        mid := (l + r) >> 1
        down(i)
        ans := 0
        if jobl <= mid {
            ans = max(ans,query(jobl, jobr, l, mid, i << 1))
        }
        if jobr > mid {
            ans = max(ans,query(jobl, jobr, mid + 1, r, i << 1 | 1))
        }
        return ans
    }

    n := collect(positions)
    build(1, n, 1)
    var ans []int
    maxheight := 0
    for _, pos := range positions {
        l := rank(n, pos[0])
        r := rank(n, pos[0] + pos[1] - 1)
        h := query(l, r, 1, n, 1) + pos[1]
        maxheight = max(maxheight, h)
        ans = append(ans,maxheight)
        updateFunc(l, r, h, 1, n, 1)
    }
    return ans
}

```
---

## C++ 代码

```C++

class Solution {
public:
    vector<int> fallingSquares(vector<vector<int>>& positions) {
        const int MAXN = 2001;
        vector<int> arr(MAXN), maxh(MAXN << 2), update(MAXN << 2), change(MAXN << 2);

        auto collect = [&](vector<vector<int>>& pos) -> int {
            int idx = 0;
            for (int i = 0; i < pos.size(); i++) {
                arr[idx++] = pos[i][0];
                arr[idx++] = pos[i][0] + pos[i][1] - 1;
            }
            sort(arr.begin(), arr.begin() + idx);
            int n = 1;
            for (int i = 1; i < idx; i++) {
                if (arr[i] != arr[n - 1]) {
                    arr[n++] = arr[i];
                }
            }
            return n;
        };

        auto rank = [&](int n, int v) -> int {
            int l = 0, r = n - 1;
            int ans = 0;
            while (l <= r) {
                int mid = (l + r) >> 1;
                if (arr[mid] >= v) {
                    ans = mid;
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            }
            return ans + 1;
        };

        auto up = [&](int i) -> void {
            maxh[i] = max(maxh[i << 1], maxh[i << 1 | 1]);
        };

        auto lazy = [&](int i, int v) -> void {
            update[i] = true;
            change[i] = v;
            maxh[i] = v;
        };

        function<void(int)> down = [&](int i) -> void {
            if (update[i]) {
                lazy(i << 1, change[i]);
                lazy(i << 1 | 1, change[i]);
                update[i] = false;
            }
        };

        function<void(int, int, int)> build = [&](int l, int r, int i) -> void {
            if (l < r) {
                int mid = (l + r) >> 1;
                build(l, mid, i << 1);
                build(mid + 1, r, i << 1 | 1);
            }
            change[i] = 0;
            maxh[i] = 0;
            update[i] = false;
        };

        function<void(int, int, int, int, int, int)> updatefunc = [&](int jobl, int jobr, int jobv, int l, int r, int i) -> void {
            if (jobl <= l && r <= jobr) {
                lazy(i, jobv);
            } else {
                down(i);
                int mid = (l + r) >> 1;
                if (jobl <= mid) {
                    updatefunc(jobl, jobr, jobv, l, mid, i << 1);
                }
                if (jobr > mid) {
                    updatefunc(jobl, jobr, jobv, mid + 1, r, i << 1 | 1);
                }
                up(i);
            }
        };

        function<int(int, int, int, int, int)> query = [&](int jobl, int jobr, int l, int r, int i) -> int {
            if (jobl <= l && r <= jobr) {
                return maxh[i];
            }
            down(i);
            int ans = 0;
            int mid = (l + r) >> 1;
            if (jobl <= mid) {
                ans = max(ans, query(jobl, jobr, l, mid, i << 1));
            }
            if (jobr > mid) {
                ans = max(ans, query(jobl, jobr, mid + 1, r, i << 1 | 1));
            }
            return ans;
        };

        int n = collect(positions);
        build(1, n, 1);
        vector<int> ans;
        int maxheight = 0;
        for (int i = 0; i < positions.size(); i++) {
            int l = rank(n, positions[i][0]);
            int r = rank(n, positions[i][0] + positions[i][1] - 1);
            int h = query(l, r, 1, n, 1) + positions[i][1];
            maxheight = max(maxheight, h);
            ans.push_back(maxheight);
            updatefunc(l, r, h, 1, n, 1);
        }
        return ans;
    }
};


```
---
## Python 代码

```Python

class Solution:
    def fallingSquares(self, positions: list[list[int]]) -> list[int]:
        MAXN = 2001
        arr = [0] * MAXN
        maxh = [0] * (MAXN << 2)
        update = [False] * (MAXN << 2)
        change = [0] * (MAXN << 2)

        def collect(pos: list[list[int]]) -> int:
            idx = 0
            for i in range(len(pos)):
                arr[idx] = pos[i][0]
                idx += 1
                arr[idx] = pos[i][0] + pos[i][1] - 1
                idx += 1
            temp = arr[:idx]
            temp.sort()
            n = 1
            for i in range(1, len(temp)):
                if temp[i] != temp[n - 1]:
                    temp[n] = temp[i]
                    n += 1
            for i in range(n):
                arr[i] = temp[i]
            return n

        def rank(n: int, v: int) -> int:
            l, r = 0, n - 1
            ans = 0
            while l <= r:
                mid = (l + r) >> 1
                if arr[mid] >= v:
                    ans = mid
                    r = mid - 1
                else:
                    l = mid + 1
            return ans + 1

        def up(i: int):
            maxh[i] = max(maxh[i << 1], maxh[i << 1 | 1])

        def lazy(i: int, v: int):
            update[i] = True
            change[i] = v
            maxh[i] = v

        def down(i: int):
            if update[i]:
                lazy(i << 1, change[i])
                lazy(i << 1 | 1, change[i])
                update[i] = False

        def build(l: int, r: int, i: int):
            if l < r:
                mid = (l + r) >> 1
                build(l, mid, i << 1)
                build(mid + 1, r, i << 1 | 1)
            change[i] = 0
            maxh[i] = 0
            update[i] = False

        def updatefunc(jobl: int, jobr: int, jobv: int, l: int, r: int, i: int):
            if jobl <= l and r <= jobr:
                lazy(i, jobv)
            else:
                down(i)
                mid = (l + r) >> 1
                if jobl <= mid:
                    updatefunc(jobl, jobr, jobv, l, mid, i << 1)
                if jobr > mid:
                    updatefunc(jobl, jobr, jobv, mid + 1, r, i << 1 | 1)
                up(i)

        def query(jobl: int, jobr: int, l: int, r: int, i: int) -> int:
            if jobl <= l and r <= jobr:
                return maxh[i]
            down(i)
            mid = (l + r) >> 1
            ans = 0
            if jobl <= mid:
                ans = max(ans, query(jobl, jobr, l, mid, i << 1))
            if jobr > mid:
                ans = max(ans, query(jobl, jobr, mid + 1, r, i << 1 | 1))
            return ans

        n = collect(positions)
        build(1, n, 1)
        ans = []
        maxheight = 0
        for i in range(len(positions)):
            l = rank(n, positions[i][0])
            r = rank(n, positions[i][0] + positions[i][1] - 1)
            h = query(l, r, 1, n, 1) + positions[i][1]
            maxheight = max(maxheight, h)
            ans.append(maxheight)
            updatefunc(l, r, h, 1, n, 1)
        return ans

```
