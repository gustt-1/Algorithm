# [LeetCode3767] [选择 K 个任务的最大总分数](https://leetcode.cn/problems/maximize-points-after-choosing-k-tasks/description/)

## 题目描述
给你两个整数数组 `technique1` 和 `technique2`，长度均为 `n` ，其中 `n` 代表需要完成的任务数量。

如果第 `i` 个任务使用技巧 `1` 完成，你将获得 `technique1[i]` 分。
如果使用技巧 `2` 完成，你将获得 `technique2[i]` 分。
此外给你一个整数 `k` ，表示 必须 使用技巧 `1` 完成的 最少 任务数量。

你 **必须** 使用技巧 `1` 完成 至少 `k` 个任务（**不需要是前** `k` 个任务）。

剩余的任务可以使用 **任一** 技巧完成。

返回一个整数，表示你能获得的 **最大总分数**。




---

## 题目大意


---

## 输入




## 输出

> 

---

## 我的思路

**贪心/排序**

> 我们先假设**全部**选择使用`技巧 1` ，即先满足至少 `k` 个任务的要求。然后我们可以进行反悔，我们创建个数组 $reward$ 表示 `使用技巧2` - `使用技巧1` 的**差值**，如果`大于 0` 即当前位置 $i$ 使用 `技巧2` 是**优于** `技巧 1` 的。我们把`大于 0` 的加入到 $reward$ 中

> 然后我们选择 $reward$ 中最大的 $min(n - k, len(technique1))$ 个任务，即反悔使用技巧 $2$ 。

---

## 时间复杂度

$O()$

---

## 空间复杂度

$O()$

---

## Go 代码

```Go
func maxPoints(technique1 []int, technique2 []int, k int) (ans int64) {
    n := len(technique1)
    reward := []int{}
    for i, x := range technique1 {
        ans += int64(x)
        d := technique2[i] - x
        if d > 0 {
            reward = append(reward, d)
        }
    }
    sort.Slice(reward, func(i, j int) bool {
        return reward[i] > reward[j]
    })
    for _, x := range reward[:min(n-k, len(reward))] {
        ans += int64(x)
    }
    return
}
```
---

## C++ 代码

```C++
class Solution {
public:
    long long maxPoints(vector<int>& a, vector<int>& b, int k) {
        int n = a.size();
        long long ans = 0;
        vector<int> diff;

        for (int i = 0; i < n; i++) {
            ans += a[i];
            int d = b[i] - a[i];
            if (d > 0) {
                diff.push_back(d);
            }
        }

        ranges::sort(diff, greater());
        int limit = min(n - k, (int) diff.size());
        ans += reduce(diff.begin(), diff.begin() + limit, 0LL);
        return ans;
    }
};
```
---
## Python 代码

```Python
class Solution:
    def maxPoints(self, technique1: List[int], technique2: List[int], k: int) -> int:
        d = sorted((y - x for x, y in zip(technique1, technique2) if y > x), reverse=True)
        return sum(technique1) + sum(d[:len(technique1) - k])
```

---

## JavaScript 代码

```JavaScript
```
