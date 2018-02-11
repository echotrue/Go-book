Go有一个预先定义的error接口类型

```
type error interface {
    Error() string
}
```

错误值用来表示异常状态；errors 包中有一个 errorString 结构体实现了 error 接口。当程序处于错误状态时可以用`os.Exit(1)`

来中止运行。

##### 定义错误





