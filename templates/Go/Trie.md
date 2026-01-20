# 前缀树(Trie)


### 初始化: `trie := NewTrie()`

```
type Node struct {
    son [26]*Node
    end bool
}

type Trie struct {
    root *Node
}

// 改成 NewTrie 并返回指针
func NewTrie() *Trie {
    return &Trie{root: &Node{}}
}

func (this *Trie) Insert(word string) {
    cur := this.root
    for _, c := range word {
        c -= 'a'
        if cur.son[c] == nil {
            cur.son[c] = &Node{}
        }
        cur = cur.son[c]
    }
    cur.end = true
}

func (this *Trie) find(word string) int {
    cur := this.root
    for _, c := range word {
        c -= 'a'
        if cur.son[c] == nil {
            return 0
        }
        cur = cur.son[c]
    }
    if cur.end {
        return 2
    }
    return 1
}

func (this *Trie) Search(word string) bool {   // 找到完整的
    return this.find(word) == 2
}

func (this *Trie) StartsWith(prefix string) bool {  // 找到前缀
    return this.find(prefix) != 0
}
```
