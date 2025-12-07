# 质数筛

## $isprime[x] == true$ 为`质数(素数)` ，根据数据量指定 $mx$ 大小

**$init$ 中调用函数，每个包中的每个 $init()$ 都会`执行一次`，减少时间开销**

### 欧拉筛
---  
``` 欧拉筛
const mx = 5000001

var isprime [mx]bool
var primes []int

func sieve() {
	for i := 2; i < mx; i++ {
		isprime[i] = true
	}

	primes = []int{}
	for i := 2; i < mx; i++ {
		if isprime[i] {
			primes = append(primes, i)
		}
		for _, p := range primes {
			if i*p >= mx {
				break
			}
			isprime[i*p] = false
			if i%p == 0 {
				break
			}
		}
	}
}

func init() {
    sieve()
}
```

---

### 埃氏筛
```  埃氏筛
const mx = 5000001
var isprime [mx]bool
var primes []int

func sieve() {
	for i := 2; i < mx; i++ {
		isprime[i] = true
	}

	for i := 2; i < mx; i++ {
		if isprime[i] {
			primes = append(primes, i)
		}
		for j := i * i; j < mx; j += i {
			isprime[j] = false
		}
	}
}

func init() {
	sieve()
}
```
