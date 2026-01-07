# [LeetCode2543] [判断一个点是否可以到达](https://leetcode.cn/problems/check-if-point-is-reachable/description/)

## 题目描述
给你一个无穷大的网格图。一开始你在 $(1, 1)$ ，你需要通过有限步移动到达点 $(targetX, targetY)$ 。

每一步 ，你可以从点 $(x, y)$ 移动到以下点之一：

$(x, y - x)$

$(x - y, y)$

$(2 * x, y)$

$(x, 2 * y)$

给你两个整数 $targetX$ 和 $targetY$ ，分别表示你最后需要到达点的 $X $和 $Y$ 坐标。如果你可以从 $(1, 1)$ 出发到达这个点，请你返回 $true$ ，否则返回 $false$ 。



---

## 题目大意


---

## 输入




## 输出

> 

---

## 我的思路

**GCD/裴蜀定理**


- 反着思考，我们从终点返回起点，根据裴蜀定理， $(targetX, targetY)$ 可以到达 $(1, 1)$ 当且仅当 $gcd(targetX, targetY) = 1$ 。而 $x - y$ 和 $y - x$ 这两个操作对 gcd 没有影响，所以我们只需要考虑 $2 * x$ 和 $2 * y$ 这两个操作。对于除以 2 这个操作， gcd(ma, mb) = m * gcd(a, b) 最终要使得 $gcd(targetX, targetY) = 1$ 那么等价于 $gcd(targetX, targetY)$ 只包含 $2$ 这个因子，即 $gcd(targetX, targetY)$ 是 $2$ 的幂。判断是否为 $2$ 的幂可以判断 $g$ & $(g - 1) == 0$ ，其中 $(g = gcd(targetX, targetY))$

> 比如 $gcd(x, y) == 4 * gcd(x/4, y)$ 或者 $gcd(x, y) == 8 *gcd(x/4, y/2)$ 。


---

## 时间复杂度

$O()$ 

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func isReachable(targetX int, targetY int) bool {
    g := gcd(targetX, targetY)
    if g & (g - 1) == 0 {
        return true
    } else {
        return false
    }
}

func gcd(a, b int) int {
	for a != 0 {
		a, b = b%a, a
	}
	return b
}
```
---

## C++ 代码

```C++
class Solution {
public:
    bool isReachable(int targetX, int targetY) {
        int g = gcd(targetX, targetY);
        return (g & (g - 1)) == 0;
    }
};
```
---
## Python 代码

```Python
class Solution:
    def isReachable(self, targetX: int, targetY: int) -> bool:
        g = gcd(targetX, targetY)
        return (g & (g - 1)) == 0
```

---

## JavaScript 代码

```JavaScript
/**
 * @param {number} targetX
 * @param {number} targetY
 * @return {boolean}
 */
var isReachable = function(targetX, targetY) {
    const gcd = (a, b) => {
        while (b !== 0) {
            a %= b;
            [a, b] = [b, a];
        }
        return a;
    };
    const g = gcd(targetX, targetY);
    return (g & (g - 1)) === 0;
};
```
