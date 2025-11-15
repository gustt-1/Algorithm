# [3234] [统计 1 显著的字符串的数量](https://leetcode.cn/problems/count-the-number-of-substrings-with-dominant-ones/description/)

## 题目描述
给你一个二进制字符串 `s` 。

请你统计并返回其中 `1 显著` 的 子字符串 的数量。

如果字符串中 `1` 的数量 大于或等于 `0` 的数量的 平方，则认为该字符串是一个 `1` 显著 的字符串 。



---


## 输入

> 


## 输出

> 

---

## 我的思路

**思维/枚举**

> 

---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func numberOfSubstrings(s string) (ans int) {
    total1 := 0
    pos0 := []int{-1}    // 哨兵，避免讨论负数下标， 全为 1 时，那么 r - (-1)就是数组长度
    for r, c := range s {     // 枚举 r 作为右端点，内层遍历左端点
        if c == '0' {
            pos0 = append(pos0, r)   // pos0 存放 0 的下标
        } else {
            total1++
            ans += r - pos0[len(pos0)-1]   // 一定是满足的，r ~ pos0[len(pos0)-1] 一定是没有 0 的
            // pos0[len(pos0)-1] 即为上一个 0 的位置
        }

        m := len(pos0)
        // (m-l) 即为 l ~ r 位置 0 的个数
        // (m-l)*(m-l) <= total1 如果 l ~ r 的 0 的个数都已经比0 ~ r 的 1 的个数要多了，那么直接退出去后面找更多的 1
        for l := m - 1; l > 0 && (m-l)*(m-l) <= total1; l-- {
            p, q := pos0[l-1], pos0[l] // p 为上上个 0 的位置， q 为上个 0 的位置
            cnt0 := m - l  // pos0 的长度减去下标即为 0 的个数，cnt0 的位置即为 q 的位置
            cnt1 := r - q + 1 - cnt0  // 子串 r ~ l 中 1 的个数即为 长度减去 0 的个数
            if cnt0*cnt0 <= cnt1 {
                // 如果 q 位置满足条件，那么 p+1 ~ r 为左端点都是可以的
                ans += q-p
            } else {
                // 如果不满足，那么我们还需要 cnt0^2 - cnt1 个 1
                // 那么分段即为 q -----  |    need    | - p 那么 满足的是 q+1 ~ need的左端点
                need := cnt0*cnt0 - cnt1
                ans += max(q-need-p, 0)
            }
        }
    }
    return
}
```
---

## C++ 代码

```C++
class Solution {
public:
    int numberOfSubstrings(string s) {
        int ans = 0;
        int total1 = 0;
        vector<int> pos0 = {-1}; // 哨兵，避免讨论负数下标，全为 1 时，那么 r - (-1) 就是数组长度

        for (int r = 0; r < s.size(); r++) { // 枚举 r 作为右端点，内层遍历左端点
            char c = s[r];
            if (c == '0') {
                pos0.push_back(r); // pos0 存放 0 的下标
            } else {
                total1++;
                // 一定是满足的，r ~ pos0.back() 一定是没有 0 的
                // pos0.back() 即为上一个 0 的位置
                ans += r - pos0.back();
            }

            int m = pos0.size();
            // (m-l) 即为 l ~ r 位置 0 的个数
            // (m-l)*(m-l) <= total1 如果 l~r 的 0 的个数都已经比 0~r 的 1 的个数要多，
            // 那么直接退出去后面找更多的 1
            for (int l = m - 1; l > 0 && (m - l) * (m - l) <= total1; l--) {
                int p = pos0[l - 1], q = pos0[l]; // p 为上上个 0 的位置，q 为上个 0 的位置
                int cnt0 = m - l;                 // 从 q 到 r 的 0 的个数
                int cnt1 = (r - q + 1) - cnt0;    // q~r 中 1 的个数 = 长度 - 0

                if (cnt0 * cnt0 <= cnt1) {
                    // 如果 q 位置满足条件，那么 p+1 ~ q 为左端点都是可以的
                    ans += q - p;
                } else {
                    // 如果不满足，那么我们还需要 cnt0^2 - cnt1 个 1
                    // 那么分段即为 q ----- | need | - p
                    // 那么满足的是 (q-need) ~ p 的区间长度
                    int need = cnt0 * cnt0 - cnt1;
                    ans += max(q - need - p, 0);
                }
            }
        }
        return ans;
    }
};
```
---
## Python 代码

```Python
max = lambda a, b: b if b > a else a
class Solution:
    def numberOfSubstrings(self, s: str) -> int:
        ans = 0
        total1 = 0
        pos0 = [-1]  # 哨兵，避免讨论负数下标，全为 1 时，那么 r - (-1) 就是数组长度

        for r, c in enumerate(s):  # 枚举 r 作为右端点，内层遍历左端点
            if c == '0':
                pos0.append(r)  # pos0 存放 0 的下标
            else:
                total1 += 1
                # 一定是满足的，r ~ pos0[len(pos0)-1] 一定是没有 0 的
                # pos0[len(pos0)-1] 即为上一个 0 的位置
                ans += r - pos0[-1]

            m = len(pos0)
            # (m-l) 即为 l ~ r 位置 0 的个数
            # (m-l)*(m-l) <= total1 如果 l ~ r 的 0 的个数都已经比 0~r 的 1 的个数要多了，
            # 那么直接退出去后面找更多的 1
            l = m - 1
            while l > 0 and (m - l) * (m - l) <= total1:
                p, q = pos0[l - 1], pos0[l]  # p 为上上个 0 的位置，q 为上个 0 的位置
                cnt0 = m - l  # pos0 的长度减去下标即为 0 的个数，cnt0 的位置即为 q 的位置
                cnt1 = (r - q + 1) - cnt0  # 子串 q ~ r 中 1 的个数即为长度减去 0 的个数

                if cnt0 * cnt0 <= cnt1:
                    # 如果 q 位置满足条件，那么 p+1 ~ q 为左端点都是可以的
                    ans += q - p
                else:
                    # 如果不满足，那么我们还需要 cnt0^2 - cnt1 个 1
                    # 那么分段即为 q ----- | need | - p
                    # 那么满足的是 (q-need) ~ p 的区间长度
                    need = cnt0 * cnt0 - cnt1
                    ans += max(q - need - p, 0)

                l -= 1

        return ans
```

---

## JavaScript 代码

```JavaScript
/**
 * @param {string} s
 * @return {number}
 */
var numberOfSubstrings = function(s) {
    let ans = 0;
    let total1 = 0;
    let pos0 = [-1]; // 哨兵，避免讨论负数下标，全为 1 时，那么 r - (-1) 就是数组长度

    for (let r = 0; r < s.length; r++) { // 枚举 r 作为右端点
        const c = s[r];
        if (c === '0') {
            pos0.push(r); // pos0 存放 0 的下标
        } else {
            total1++;
            // 一定满足 1 显著（0 个数为 0）
            ans += r - pos0[pos0.length - 1];
        }

        const m = pos0.length;
        // (m-l) 即为 l ~ r 位置 0 的个数
        let l = m - 1;
        while (l > 0 && (m - l) * (m - l) <= total1) {
            const p = pos0[l - 1], q = pos0[l]; // p 为上上个 0，q 为上个 0
            const cnt0 = m - l; // q ~ r 的 0 个数
            const cnt1 = (r - q + 1) - cnt0; // q~r 中 1 的个数 = 长度 - 0

            if (cnt0 * cnt0 <= cnt1) {
                ans += q - p; // p+1 ~ q
            } else {
                // 如果不满足，那么我们还需要 cnt0^2 - cnt1 个 1
                // 那么分段即为 q ----- | need | - p
                // 那么满足的是 (q-need) ~ p 的区间长度
                const need = cnt0 * cnt0 - cnt1;
                ans += Math.max(q - need - p, 0);
            }
            l--;
        }
    }
    return ans;
};
```
