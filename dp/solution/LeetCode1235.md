# [LeetCode1235] [规划兼职工作](https://leetcode.cn/problems/maximum-profit-in-job-scheduling/description/)

## 题目描述
你打算利用空闲时间来做兼职工作赚些零花钱。

这里有 `n` 份兼职工作，每份工作预计从 `startTime[i]` 开始到 `endTime[i]` 结束，报酬为 `profit[i]` 。

给你一份兼职工作表，包含开始时间 `startTime` ，结束时间 `endTime` 和预计报酬 `profit` 三个数组，请你计算并返回可以获得的最大报酬。

注意，时间上出现重叠的 `2` 份工作不能同时进行。

如果你选择的工作在时间 `X` 结束，那么你可以立刻进行在时间 `X` 开始的下一份工作。


---

## 题目大意



---

## 输入




## 输出

> 

---

## 我的思路

**二分查找优化DP**

> 我们将每个兼职工作按照**结束时间**进行**从小到大**排序。

> 定义 $f[i]$ 表示**前** $i$ 份兼职工作可以获得的最大报酬。

> 来到当前 $i$ 兼职工作，我们可以选择**不做**这份工作，那么可以直接从 $f[i-1]$ 转移过来。

> 如果选择**做**这份工作，那么我们需要找到**最后一个**结束时间**小于等于** $startTime[i]$ 的兼职工作 $j$ ，那么可以从 $f[j]$ 转移过来，转移的代价为 $profit[i]$ 。由于我们已经将兼职工作按照**结束时间**进行了排序，所以可以使用**二分查找优化**。

> 所以转移方程为 $f[i] = max(f[i-1], f[j] + profit[i])$

**因为转移有 $f[i-1]$ ，为了避免讨论负数下标我们可以将f数组的长度设为 $n+1$ ， $f[0]$ 表示不做任何兼职工作的情况，所以转移方程为 $f[i] = max(f[i-1], f[j+1] + profit[i])$**


---

## 时间复杂度

$O(n)$ 

---

## 空间复杂度

$O(1)$

---

## Go 代码

```Go
func jobScheduling(startTime []int, endTime []int, profit []int) int {
    n := len(profit)
    f := make([]int, n+1)
    type job struct {
        s, e, p int
    }
    jobs := make([]job, n)
    for i, st := range startTime {
        jobs[i] = job{st, endTime[i], profit[i]}
    }
    sort.Slice(jobs, func(i, j int) bool {
        return jobs[i].e < jobs[j].e
    })
    for i, job := range jobs {
        idx := sort.Search(i, func(j int) bool {
            return jobs[j].e > jobs[i].s
        })
        f[i+1] = max(f[i], f[idx]+job.p)
    }
    return f[n]
}

```
---

## C++ 代码

```C++

```
---
## Python 代码

```Python
class Solution:
    def jobScheduling(self, startTime: List[int], endTime: List[int], profit: List[int]) -> int:
        n = len(profit)
        f = [0] * (n + 1)
        
        jobs = []
        # jobs[0] 是 startTime, jobs[1] 是 endTime, jobs[2] 是 profit
        for i in range(n):
            jobs.append((startTime[i], endTime[i], profit[i]))
            
        jobs.sort(key=lambda x: x[1])
        
        for i, job in enumerate(jobs):
            idx = bisect.bisect_right(jobs, job[0], hi=i, key=lambda x: x[1])
            f[i+1] = max(f[i], f[idx] + job[2])
            
        return f[n]
```

---

## JavaScript 代码

```JavaScript
```
