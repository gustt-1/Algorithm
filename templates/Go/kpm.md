# KMP

返回值是 $target$ 串中第一个可以匹配 $need$ 的下标

```
func kmp(need string, target string) int {
    // target 要匹配的，need 模式串
    n, m := len(need), len(target)
    // cnt := 0    // 统计匹配的个数
    if m == 0 {
        return 0
    }

    // 构建 target 的 next 数组
    next := make([]int, m)
    j := 0 // 前缀长度
    for i := 1; i < m; i++ {
        for j > 0 && target[i] != target[j] {
            j = next[j-1] // 回退到前一个最长前缀
        }
        if target[i] == target[j] {
            j++
        }
        next[i] = j
    }

    // 匹配 need
    j = 0
    for i := 0; i < n; i++ {
        for j > 0 && need[i] != target[j] {
            j = next[j-1] // 回退到前一个最长前缀
        }
        if need[i] == target[j] {
            j++
        }
        if j == m { // 匹配成功
            // cnt++
            return i - m + 1
        }
    }
    // return cnt
    return -1
}
```
