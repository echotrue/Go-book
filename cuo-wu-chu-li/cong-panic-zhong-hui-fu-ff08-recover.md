正如名字一样，这个（recover）内建函数被用于从 panic 或 错误场景中恢复：让程序可以从 panicking 重新获得控制权，停止终止过程进而恢复正常执行。

`recover`只能在 defer 修饰的函数中使用：用于取得 panic 调用中传递过来的错误值，如果是正常执行，调用`recover`会返回 nil，且没有其它效果。

总结：panic 会导致栈被展开直到 defer 修饰的 recover\(\) 被调用或者程序中止。

这是一个展示 panic，defer 和 recover 怎么结合使用的完整例子：

