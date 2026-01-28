# 大/小根堆

## 创建

`h := &minHeap{}`

`heap.Init(h)`

## 返回

在 $Go$ 中，`heap.Pop(h)` 的**返回值类型**是 `any（以前叫 interface{}）`，因为 `container/heap 包`不知道具体堆元素类型，只保证是你 `Push` 进去的元素。因此如果你的堆存放的是`自定义类型 pair`，就需要使用类型断言 `.(pair)` 将 `any` 转回 `pair`，**才能访问其字段**，例如 `res := heap.Pop(h).(pair)`。如果堆存放的是**普通 `int`**，类型断言就写 `.(int)`。


### `x := heap.Pop(h).(int)`，这时 `x` 就是 `当前堆`的`堆顶元素`，并且被弹出（Pop 会移除堆顶）。所有操作结束后最终的堆为 `*(h)`，`(*h)[0]`即是堆顶。

### $pop: heap.Pop(h).(int)$

### $push: heap.Push(h, val)$ 

### 取长度:$h.Len()$ 

### 取堆顶:$(*h)[0]$

---

## 大根堆 Pop出来的最大
```   Go
type maxHeap []int
func (h maxHeap) Len() int           { return len(h) }
func (h maxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h maxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *maxHeap) Push(x any)        { *h = append(*h, x.(int)) }
func (h *maxHeap) Pop() any {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[:n-1]
    return x
}
```

## 小根堆 Pop出来的最小
```
type minHeap []int 
func (h minHeap) Len() int           { return len(h) }
func (h minHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h minHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *minHeap) Push(x any)        { *h = append(*h, x.(int)) }
func (h *minHeap) Pop() any {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[:n-1]
    return x
}
```

## 若要根据别的来排序就按照下面的：

``` Go
type pair struct {
    num  int // 元素值
    freq int // 自定义比较权值
}

// ===== 小根堆模板（堆顶最小）=====
type minHeap []pair

func (h minHeap) Len() int           { return len(h) }
func (h minHeap) Less(i, j int) bool { return h[i].freq < h[j].freq } // 小根堆
func (h minHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *minHeap) Push(x any)        { *h = append(*h, x.(pair)) }   //注意x.(pair))
func (h *minHeap) Pop() any {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[:n-1]
    return x
}

// ===== 大根堆模板（堆顶最大）=====
type maxHeap []pair

func (h maxHeap) Len() int           { return len(h) }
func (h maxHeap) Less(i, j int) bool { return h[i].freq > h[j].freq } // 大根堆
func (h maxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *maxHeap) Push(x any)        { *h = append(*h, x.(pair)) }
func (h *maxHeap) Pop() any {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[:n-1]
    return x
}
```
