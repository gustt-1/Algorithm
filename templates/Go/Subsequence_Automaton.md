https://leetcode.cn/problems/is-subsequence/solutions/2813031/jian-ji-xie-fa-pythonjavaccgojsrust-by-e-mz22/

如果有 $k$ 个字符串 $s_1 ,s_2, … , s_k$ ，每个字符串都需要遍历 $t$ ，都需要花费 $O(n)$ 的时间，太慢了。下面介绍一个更快的做法，在预处理后，每个字符串都只需要 $O(m)$ 的时间，其中 $m$ 为 $s_i$ 的长度。

首先，上面的方法慢在哪？如果 $s$ 中的字母都集中在 t 的末尾（或者相隔很远），那么我们会浪费大量时间在遍历 $t$ 上。
能否「一步到位」呢？比如 $s=abc，t=ahbgdc$ ，能否 $O(1)$ 算出下一个字母 $a$ 的位置，下一个字母 $b$ 的位置，下一个字母 $c$ 的位置？
可以。定义 $nxt[i][c]$ 表示 t 中下标 $≥i$ 的最近字母 $c$ 的下标。如果 $c$ 不存在，则规定 $nxt[i][c]=n$ ，用 $n$ 表示没找到。

以 $s=abc$ ， $t=ahbgdc$ 为例：

预处理字符串 $t$ ，得到 $nxt$ 数组，做法见后文。
初始化 $i=−1$ 。
遍历字符串 $s$ 。

找 $i$ 右边最近字母 $s[0]=a$ 的下标，直接看 $nxt[i+1][a]$ 是多少，即 $0$ ，更新 $i=0$ 。

找 $i$ 右边最近字母 $s[1]=b$ 的下标，直接看 $nxt[i+1][b]$ 是多少，即 $2$ ，更新 $i=2$ 。

找 $i$ 右边最近字母 $s[2]=c$ 的下标，直接看 $nxt[i+1][c]$ 是多少，即 $5$ ，更新 $i=5$ 。

字符串 s 正常遍历完毕，说明 $s$ 是 $t$ 的子序列。如果遍历中途 $i=n$ ，则 $s$ 不是 $t$ 的子序列。
那么，如何预处理 $nxt[i][c]$ 呢？这可以用**动态规划**：


如果 $t[i]=c$ ，根据定义， $nxt[i][c]=i$ 。
如果 $t[i] !=c$ ，问题变成 $t$ 中下标 $≥i+1$ 的最近字母 $c$ 的下标，即 $nxt[i][c]=nxt[i+1][c]$ 。
初始值 $t[n][c]=n$ 。

作者：灵茶山艾府
链接：https://leetcode.cn/problems/is-subsequence/solutions/2813031/jian-ji-xie-fa-pythonjavaccgojsrust-by-e-mz22/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

``` Go
func isSubsequence(s string, t string) bool {
    n := len(t)
    nxt := make([][26]int, n+1)
    for j := range nxt[n] {
        nxt[n][j] = n
    }
    for i := n - 1; i >= 0; i-- {
        nxt[i] = nxt[i+1]
        nxt[i][t[i]-'a'] = i
    }

    i := -1
    for _, b := range s {
        i = nxt[i+1][b-'a']
        if i == n {
            return false
        }
    }
    return true
}
```
