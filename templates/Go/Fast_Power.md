## $power(base, n)$ 为 $base^n$


``` GoLang
func power(base int64, n int64) int64 {
    res := int64(1)
    for n > 0 {
        if n&1 == 1 {
            res *= base
        }
        base *= base
        n >>= 1
    }
    return res
}
```
