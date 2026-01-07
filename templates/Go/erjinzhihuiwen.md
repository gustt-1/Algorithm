```
var palindromes []int

// 预处理二进制回文数
func init() {
	const mx = 5000
	const base = 2

	// 哨兵，0 也是回文数
	palindromes = append(palindromes, 0)

outer:
	for pw := 1; ; pw *= base {
		// 生成奇数长度回文数
		for i := pw; i < pw*base; i++ {
			x := i
			for t := i / base; t > 0; t /= base {
				x = x*base + t%base
			}
			if x > mx {
				break outer
			}
			palindromes = append(palindromes, x)
		}

		// 生成偶数长度回文数
		for i := pw; i < pw*base; i++ {
			x := i
			for t := i; t > 0; t /= base {
				x = x*base + t%base
			}
			if x > mx {
				break outer
			}
			palindromes = append(palindromes, x)
		}
	}

	// 哨兵，5049 是大于 5000 的第一个二进制回文数
	palindromes = append(palindromes, 5049)
}

// 作者：灵茶山艾府
// 链接：https://leetcode.cn/problems/minimum-operations-to-make-binary-palindrome/solutions/3850779/fei-bao-li-zuo-fa-pythonjavacgo-by-endle-3gi2/
// 来源：力扣（LeetCode）
// 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
