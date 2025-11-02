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
