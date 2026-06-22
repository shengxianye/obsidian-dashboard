# Go 并发模式

## 基础：Goroutine + Channel
```go
ch := make(chan int, 10)
go func() {
    ch <- 42
}()
val := <-ch
```

## 常用模式

### Fan-out / Fan-in
多个 goroutine 读同一个 channel（fan-out），多个 channel 结果汇聚到一个（fan-in）。

### Worker Pool
```go
func worker(id int, jobs <-chan Job, results chan<- Result) {
    for j := range jobs {
        results <- process(j)
    }
}
```

### Context 取消
```go
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()
```

## 注意事项
- 不要通过共享内存通信，要通过通信共享内存
- 注意 goroutine 泄漏：确保有退出路径
