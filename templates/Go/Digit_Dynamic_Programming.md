# 数位DP


``` Python
# 代码示例：返回 [low, high] 中的恰好包含 target 个 0 的数字个数
# 比如 digitDP(0, 10, 1) == 2
# 要点：我们统计的是 0 的个数，需要区分【前导零】和【数字中的零】，前导零不能计入，而数字中的零需要计入
def digitDP(low: int, high: int, target: int) -> int:
    low_s = list(map(int, str(low)))  # 避免在 dfs 中频繁调用 int()
    high_s = list(map(int, str(high)))
    n = len(high_s)
    diff_lh = n - len(low_s)

    @cache
    def dfs(i: int, cnt0: int, limit_low: bool, limit_high: bool) -> int:
        if cnt0 > target:
            return 0  # 不合法
        if i == n:
            return 1 if cnt0 == target else 0

        lo = low_s[i - diff_lh] if limit_low and i >= diff_lh else 0
        hi = high_s[i] if limit_high else 9

        res = 0
        start = lo

        # 通过 limit_low 和 i 可以判断能否不填数字，无需 is_num 参数
        # 如果前导零不影响答案，去掉这个 if block
        if limit_low and i < diff_lh:
            # 不填数字，上界不受约束
            res = dfs(i + 1, 0, True, False)
            start = 1

        for d in range(start, hi + 1):
            res += dfs(i + 1,
                       cnt0 + (1 if d == 0 else 0),  # 统计 0 的个数
                       limit_low and d == lo,
                       limit_high and d == hi)

        # res %= MOD
        return res

    return dfs(0, 0, True, True)

# 作者：灵茶山艾府
# 链接：https://leetcode.cn/discuss/post/tXLS3i/
```

## 例题：

给你正整数 $low,high$ 和 $k$ 。
如果一个数满足以下两个条件，那么它是 **美丽的** ：

(1) 偶数数位的数目与奇数数位的数目相同。

(2) 这个整数可以被 $k$ **整除**。

请你返回范围 $[low, high]$ 中美丽整数的数目。

```
class Solution:
    def numberOfBeautifulIntegers(self, low: int, high: int, k: int) -> int:
        low_s = list(map(int, str(low)))  # 避免在 dfs 中频繁调用 int()
        high_s = list(map(int, str(high)))
        n = len(high_s)
        diff_lh = n - len(low_s)

        @cache
        def dfs(i: int, diff: int, rem: int, limit_low: bool, limit_high: bool) -> int:
            # 填完了所有的数位，算合法答案
            if i == n:
                return 1 if diff == 0 and rem == 0 else 0

            lo = low_s[i - diff_lh] if limit_low and i >= diff_lh else 0
            hi = high_s[i] if limit_high else 9

            res = 0
            start = lo

            # 通过 limit_low 和 i 可以判断能否不填数字，无需 is_num 参数
            # 如果前导零不影响答案，去掉这个 if block
            if limit_low and i < diff_lh:
                # 不填数字，上界不受约束
                res = dfs(i + 1, 0, 0, True, False)   # 直接跳过当前位
                start = 1  # 开始要填数，start可以从 1 开始

            for d in range(start, hi + 1):
                res += dfs(i + 1,
                        diff + (1 if d & 1 == 0 else -1),
                        (rem * 10 + d) % k,
                        limit_low and d == lo,
                        limit_high and d == hi)

            return res

        return dfs(0, 0, 0, True, True)
```
