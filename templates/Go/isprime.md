# 质数筛

## $isprime[x] == true$ 为`质数`
```  埃氏筛
const mx = 5000001
var isprime = [mx]bool{true, true}
var primes []int

func sieve() {
    for i := 2; i < mx; i++ {
        if !isprime[i] {
            primes = append(primes, i)
            for j := i * i; j < mx; j += i {
                isprime[j] = true
            }
        }
    }
}
```
