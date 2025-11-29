# [LeetCode2376] [统计特殊整数](https://leetcode.cn/problems/count-special-integers/description/)

## 题目描述
如果一个正整数每一个数位都是 **互不相同** 的，我们称它是 **特殊整数** 。

给你一个 **正** 整数 `n` ，请你返回区间 `[1, n]` 之间特殊整数的数目。

---

## 题目大意


---

## 输入

> 


## 输出

> 

---

## 我的思路

**数位DP**

> 模板见Python


 
---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func countSpecialNumbers(n int) int {
    s := strconv.Itoa(n)
    m := len(s)
    memo := make([][1 << 10]int, m)
    for i := range memo {
        for j := range memo[i] {
            memo[i][j] = -1   // -1 表示没有计算过
        }
    }
    var dfs func(int, int, bool, bool) int
    dfs = func(i, mask int, isLimit, isNum bool) (res int) {
        if i == m {
            if isNum {
                return 1
            }
            return 0
        }
        if !isLimit && isNum {
            p := &memo[i][mask]
            if *p >= 0 {   // 计算过
                return *p
            }
            defer func() { *p = res }()   // 记忆化
        }
        if !isNum {   // 可以跳过当前数位
            res += dfs(i+1, mask, false, false)
        }
        d := 0
        if !isNum {
            d = 1    // 如果前面没有填数字，必须从 1 开始（因为不能有前导零）
        }
        hi := 9
        if isLimit {
            hi = int(s[i] - '0')  // 如果前面填的数字都和 n 的一样，那么这一位至多填数字 s[i]（否则就超过n了）
        }
        for ; d <= hi; d++ {
            if mask>>d&1 == 0 {  // d 不在 mask 中，说明之前没有填过 d
                res += dfs(i+1, mask|1<<d, isLimit && d == hi, true)
            }
        }
        return
    }
    return dfs(0, 0, true, false)
}
```
---

## C++ 代码

```C++
class Solution {
public:
    int countSpecialNumbers(int n) {
        string s = to_string(n);
        int m = s.length();
        vector<vector<int>> memo(m, vector<int>(1 << 10, -1)); // -1 表示没有计算过
        auto dfs = [&](auto&& dfs, int i, int mask, bool isLimit, bool isNum) -> int {
            if (i == m) {
                return isNum; // isNum 为 true 表示得到了一个合法数字
            }
            if (!isLimit && isNum && memo[i][mask] != -1) {
                return memo[i][mask]; // 之前计算过
            }
            int res = 0;
            if (!isNum) { // 可以跳过当前数位
                res = dfs(dfs, i + 1, mask, false, false);
            }
            // 如果前面填的数字都和 n 的一样，那么这一位至多填数字 s[i]
            int up = isLimit ? s[i] - '0' : 9;
            // 枚举要填入的数字 d
            // 如果前面没有填数字，则必须从 1 开始（因为不能有前导零）
            for (int d = isNum ? 0 : 1; d <= up; d++) {
                if ((mask >> d & 1) == 0) { // d 不在 mask 中，说明之前没有填过 d
                    res += dfs(dfs, i + 1, mask | (1 << d), isLimit && d == up, true);
                }
            }
            if (!isLimit && isNum) {
                memo[i][mask] = res; // 记忆化
            }
            return res;
        };
        return dfs(dfs, 0, 0, true, false);
    }
};
```
---
## Python 代码

```Python
class Solution:
    def countSpecialNumbers(self, n: int) -> int:
        s = str(n)
        @cache
        # is_limit = True 前面的数刚好是s[i-1], isNum = True前面填过数字
        def dfs(i: int, mask: int, is_limit: bool, isNum: bool) -> int:
            if i == len(s):
                return 1 if isNum else 0
            res = 0
            if not isNum:      # 前面没有填过数字，可以选择跳过当前数位
                res = dfs(i + 1, mask, False, False)
            lo = 0 if isNum else 1   # 如果前面没有填过数字，那么当前位要填数字的话必须从1开始
            hi = int(s[i]) if is_limit else 9  # 如果前面所有数位都和n一样，那么当前位一定要小于等于int(s[i])，否则就比n大了
            for d in range(lo, hi+1):
                if mask >> d & 1 == 0:   # d 不在 mask 中，说明之前没有填过 d
                    res += dfs(i + 1, mask | (1 << d), is_limit and d == hi, True)
            return res
        
        return dfs(0, 0, True, False)
```

---

## JavaScript 代码

```JavaScript
```
