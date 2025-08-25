# [LeetCode858] [镜面反射](https://leetcode.cn/problems/mirror-reflection/description/)
## 题目描述 

> 有一个特殊的正方形房间，每面墙上都有一面镜子。除西南角以外，每个角落都放有一个接受器，编号为 0， 1，以及 2。正方形房间的墙壁长度为 p，一束激光从西南角射出，首先会与东墙相遇，入射点到接收器 0 的距离为 q 。返回光线最先遇到的接收器的编号（保证光线最终会遇到一个接收器）。

> 1 <= q <= p <= 1000


## 我的思路
**数学（lcm）**

##

- 模拟写会很麻烦。
- 我们看到反射，就可以让它做延长，从左下发射光线，我们就假设不会反射（0 < q < p），直接穿过右边的墙壁。那么也就相当于没碰一次，我们就往右移动 p 个单位，上升 q 个单位。每往右移动一次，就相当于把墙壁左右翻转，往上移动就是将镜面上下翻转。那么当我们往右移动 m 个单位和向上移动 n 个单位，使得 $mp = nq$（即 $m:n = p:q$ 成比例）时，我们就可以遇到一个接收器。于是我们可以计算得出 $m = \tfrac{\mathrm{lcm}(p,q)}{p}, \ n = \tfrac{\mathrm{lcm}(p,q)}{q}$。可以自己模拟一下，看m,n的奇偶性。根据移动相当于墙壁翻转的特性，可以得出根据 $m$ 和 $n$ 的奇偶性可以确定接收器的位置：当 $m$ 为奇数、 $n$ 为偶数时，光线最终落在东南角，对应接收器 0；当 $m$ 与 $n$ 都为奇数时，光线落在东北角，对应接收器 1；当 $m$ 为偶数、 $n$ 为奇数时，光线落在西北角，对应接收器 2；而当 $m$ 与 $n$ 都为偶数时，光线会回到西南角，这是不可能的情况。




##
---

## 时间复杂度

O(n)

---

## 空间复杂度

O(n)

---

## Go 代码

```Go

func mirrorReflection(p int, q int) int {
    gcd := func(a, b int) int {
        for b != 0 {
            a, b = b, a%b
        }
        return a
    }

    g := gcd(p, q)
    m := p / g
    n := q / g

    if m%2 == 1 && n%2 == 1 {
        return 1
    } else if m%2 == 1 && n%2 == 0 {
        return 0
    } else if m%2 == 0 && n%2 == 1 {
        return 2
    }
    return -1
}


```
---

## C++ 代码

```C++

class Solution {
public:
    int mirrorReflection(int p, int q) {
        // auto gcd = [](int a, int b) {
        //     while (b != 0) {
        //         int tmp = b;
        //         b = a % b;
        //         a = tmp;
        //     }
        //     return a;
        // };

        int g = gcd(p, q);
        int m = p / g;
        int n = q / g;

        if (m % 2 == 1 && n % 2 == 1) return 1;
        if (m % 2 == 1 && n % 2 == 0) return 0;
        if (m % 2 == 0 && n % 2 == 1) return 2;
        return -1;
    }
};


```
---
## Python 代码

```Python

class Solution:
    def mirrorReflection(self, p: int, q: int) -> int:
        g = gcd(p, q)
        m = p // g
        n = q // g

        if m % 2 == 1 and n % 2 == 1:
            return 1
        elif m % 2 == 1 and n % 2 == 0:
            return 0
        elif m % 2 == 0 and n % 2 == 1:
            return 2
        return -1

```
