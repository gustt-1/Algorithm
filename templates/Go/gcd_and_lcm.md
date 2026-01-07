## GCD 的一些性质

> 倍数缩放： $\gcd(ma, mb) = m \cdot \gcd(a, b)$

> 差分性质： $\gcd(a, b) = \gcd(a, b - a)$（这是更相减损术的依据）

> 余数性质： $\gcd(a, b) = \gcd(a, b \pmod a)$（这是欧几里得算法的依据）

> 裴蜀定理 (Bézout's Identity)： 对于不全为 $0$ 的整数 $x, y$ ，必然存在整数 $a, b$  使得 $ax + by = \gcd(x, y)$。推广到多个数对于 $n$ 个整数系数 $a_1, a_2, \dots, a_n$，方程： $a_1x_1 + a_2x_2 + \dots + a_nx_n = m$有整数解 $(x_1, x_2, \dots, x_n)$ 的充分必要条件是： $m \text{ 是 } \gcd(a_1, a_2, \dots, a_n) \text{ 的倍数}$

```
func gcd(a, b int) int {
	for a != 0 {
		a, b = b%a, a
	}
	return b
}
```

---

```
func lcm(a, b int) int {
	return a / gcd(a, b) * b
}
```
