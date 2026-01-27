# 数学/贪心/思维/构造（数论/组合/概率期望/博弈/计算几何/随机算法）

分类：

- 数论
  - 质数筛法（埃氏筛、Miller-Rabin）
  - 最大公约数（欧几里得算法/扩展欧几里得）
  - 模运算（快速幂、模逆元、中国剩余定理）
  - 费马小定理、欧拉定理
  - 欧拉函数、约数统计
- 组合
  - 组合数计算（C(n,k)、排列）
  - 容斥原理
  - 卡特兰数
  - 生成函数
- 概率期望
  - 概率计算基础
  - 期望的线性性质
- 博弈论
  - 必胜态分析
  - Nim博弈与SG函数
  - Sprague-Grundy定理
- 计算几何
  - 点向量运算（点积、叉积）
  - 凸包算法（Graham扫描、Andrew算法）
  - 线段相交、多边形处理
- 随机算法
  - 随机数生成
  - 蒙特卡洛方法
- 贪心
- 构造



## 力扣题目（LeetCode Problems）

| 题号 | 题目名称 | 题解链接 |  类型   | 难度(LeetCodeRating) |
|------|----------|----------|----------|----------------------|
|**858**| [镜面反射](https://leetcode.cn/problems/mirror-reflection/description/)| [LeetCode858](./solution/LeetCode858.md) | 数学(LCM) | 中等(1881) |
| **2555** | [两个线段获得的最多奖品](https://leetcode.cn/problems/maximize-win-from-two-segments/description/) | [LeetCode2555](./solution/LeetCode2555.md) | 枚举技巧 | 中等(2081) |
| **1611** | [使整数变为 0 的最少操作次数](https://leetcode.cn/problems/minimum-one-bit-operations-to-make-integers-zero/) | [LeetCode1611](./solution/LeetCode1611.md) | 数学 | 困难(2345) |
| **3234** | [统计 1 显著的字符串的数量](https://leetcode.cn/problems/count-the-number-of-substrings-with-dominant-ones/description/) | [LeetCode3234](./solution/LeetCode3234.md) | 思维/枚举 | 中等(2557) | 
| **3171** | [找到按位或最接近 K 的子数组](https://leetcode.cn/problems/find-subarray-with-bitwise-or-closest-to-k/description/) | [LeetCode3171](./solution/LeetCode3171.md) | 思维(LogTrick) | 困难(2163) |
| **3767** | [选择 K 个任务的最大总分数](https://leetcode.cn/problems/maximize-points-after-choosing-k-tasks/description/) | [LeetCode3767](./solution/LeetCode3767.md) | 贪心/排序 | 中等 |
| **2681** | [英雄的力量](https://leetcode.cn/problems/power-of-heroes/description/) | [LeetCode2681](./solution/LeetCode2681.md) | 贡献法 | 困难(2060) |
| **2543** | [判断一个点是否可以到达](https://leetcode.cn/problems/check-if-point-is-reachable/description/) | [LeetCode2543](./solution/LeetCode2543.md) | gcd性质/裴蜀定理 | 困难(2221) |

---

## Codeforces 题目（Codeforces Problems）

| 题号 | 题目名称 | 题解链接 | 类型 | 难度(CodeforcesRating) |
|------|----------|----------|------|------------------------|
| **1585C** |  [Minimize Distance](https://codeforces.com/problemset/problem/1585/C) |  [cf1585C](./solution/cf1585C.md) |  贪心  | 1300 |
| **2053C** | [Bewitching Stargazer](https://codeforces.com/problemset/problem/2053/C) | [cf2053C](./solution/cf2053C.md) | 位运算/数学 | 1500 |
| **2129B** | [Stay or Mirror](https://codeforces.com/problemset/problem/2129/B) | [cf2129B](./solution/cf2129B.md) | 贪心 | 1600 |
| **cf2044E** | [Insane Problem](https://codeforces.com/problemset/problem/2044/E) | [cf2044E](./solution/cf2044E.md) | 数学 | 1300 |
| **cf911D** | [Inversion Counting](https://codeforces.com/contest/911/problem/D) | [cf911D](./solution/cf911D.md) | 数学 | 1800 |
| **cf1893B** | [Neutral Tonality](https://codeforces.com/problemset/problem/1893/B) | [cf1893B](./solution/cf1893B.md) | 贪心/构造/最长递增子序列（LIS） | 1700 |
| **cf2137E** | [Mexification](https://codeforces.com/problemset/problem/2137/E) | [cf2137E](./solution/cf2137E.md) | 思维 | 1500 |
| **cf1982D** | [Beauty of the mountains](https://codeforces.com/problemset/problem/1982/D) | [cf1982D](./solution/cf1982D.md) | 裴蜀定理/二维前缀和/数论 | 1700 |
| **cf1766D** | [Lucky Chains](https://codeforces.com/problemset/problem/1766/D) | [cf1766D](./solution/cf1766D.md) | 数论（gcd) | 1600 |
| **cf2140C** | [Ultimate Value](https://codeforces.com/problemset/problem/2140/C) | [cf2140C](./solution/cf2140C.md) | 贪心 | 1500 |
| **cf1526B** | [I Hate 1111](https://codeforces.com/problemset/problem/1526/B) | [cf1526B](./solution/cf1526B.md) | 数论 | 1400 |
| **cf1862D** | [Ice Cream Balls](https://codeforces.com/problemset/problem/1862/D) | [cf1862D](./solution/cf1862D.md) | 数学/一元二次方程 | 1300 |
| **cf2121E** | [Sponsor of Your Problems](https://codeforces.com/contest/2121/problem/E) | [cf2121E](./solution/cf2121E.md) | 贪心 | 1500 |
| **cf1929C** | [Sasha and the Casino](https://codeforces.com/problemset/problem/1929/C) | [cf1929C](./solution/cf1929C.md) | 数学 | 1400 | 
| **cf2110C** | [Racing](https://codeforces.com/problemset/problem/2110/C) | [cf2110C](./solution/cf2110C.md) | 反悔贪心 | 1400 | 
| **cf1790D** | [Matryoshkas](https://codeforces.com/problemset/problem/1790/D) | [cf1790D](./solution/cf1790D.md) | 贪心 | 1200 |
| **cf2123D** | [Binary String Battle](https://codeforces.com/contest/2123/problem/D) | [cf2123D](./solution/cf2123D.md) | 思维/贪心 | 1400 |
| **cf2147C** | [Rabbits](https://codeforces.com/problemset/problem/2147/C) | [cf2147C](./solution/cf2147C.md) | 思维/贪心 | 1500 |
| **cf1860C** | [Game on Permutation](https://codeforces.com/problemset/problem/1860/C) | [cf1860C](./solution/cf1860C.md) | 博弈/DP | 1400 |
| **cf2157C** | [Meximum Array 2](https://codeforces.com/problemset/problem/2157/C) | [cf2157C](./solution/cf2157C.md) | 构造 | 1400 |
| **cf1838C** | [No Prime Differences](https://codeforces.com/problemset/problem/1838/C) | [cf1838C](./solution/cf1838C.md) | 构造 | 1500 |
| **cf2137D** | [Replace with Occurrences](https://codeforces.com/problemset/problem/2137/D) | [cf2137D](./solution/cf2137D.md) | 构造 | 1200 |
| **cf2084C** | [You Soared Afar With Grace](https://codeforces.com/problemset/problem/2084/C) | [cf2084C](./solution/cf2084C.md) | 构造 | 1400 |
| **cf2055C** | [The Trail](https://codeforces.com/problemset/problem/2055/C) | [cf2055C](./solution/cf2055C.md) | 构造 | 1400 |
| **cf1790E** | [Vlad and a Pair of Numbers](https://codeforces.com/problemset/problem/1790/E) | [cf1790E](./solution/cf1790E.md) | 构造/思维/位运算 | 1400 |
| **cf1837D** | [Bracket Coloring](https://codeforces.com/problemset/problem/1837/D) | [cf1837D](./solution/cf1837D.md) | 构造/贪心 | 1400 |
| **cf870C** | [Maximum splitting](https://codeforces.com/problemset/problem/870/C) | [cf870C](./solution/cf870C.md) | 数论 | 1300 |
| **cf2165A** | [Cyclic Merging](https://codeforces.com/problemset/problem/2165/A) | [cf2165A](./solution/cf2165A.md) | 思维 | 1300 |
| **cf2108B** | [SUMdamental Decomposition](https://codeforces.com/contest/2108/problem/B) | [cf2108B](./solution/cf2108B.md) | 位运算/构造 | 1300 |
| **cf1696C** | [Fishingprince Plays With Array](https://codeforces.com/problemset/problem/1696/C) | [cf1696C](./solution/cf1696C.md) | 归一化思想 | 1400 |
| **cf1903C** | [Theofanis' Nightmare](https://codeforces.com/problemset/problem/1903/C) | [cf1903C](./solution/cf1903C.md) | 边际收益贪心 | 1400 |
| **cf1682C** | [LIS or Reverse LIS?](https://codeforces.com/problemset/problem/1682/C) | [cf1682C](./solution/cf1682C.md) | 贡献法 | 1400 |
| **cf1872E** | [Data Structures Fan](https://codeforces.com/problemset/problem/1872/E) | [cf1872E](./solution/cf1872E.md) | 思维/异或性质 | 1500 |
