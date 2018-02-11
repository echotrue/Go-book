Go有一个预先定义的error接口类型

```
type error interface {
    Error() string
}
```

错误值用来表示异常状态；errors 包中有一个 errorString 结构体实现了 error 接口。当程序处于错误状态时可以用`os.Exit(1)`

来中止运行。

##### 定义错误

任何时候你需要一个新的错误类型，都可以用errors包的errors.New函数接收合适的错误信息来创建

```
func Sqrt(f float64) (float64, error) {
    if f < 0 {
        return 0, errors.New ("math - square root of negative number")
    }
   // implementation of Sqrt
}

if f, err := Sqrt(-1); err != nil {
    fmt.Printf("Error: %s\n", err)
}
```

在大部分情况下，自定义错误类型很有意义，可以包含错误信息以外的其他有用信息

