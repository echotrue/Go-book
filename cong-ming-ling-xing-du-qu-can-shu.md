os 包中有一个 string 类型的切片变量`os.Args`，用来处理一些基本的命令行参数，它在程序启动后读取命令行输入的参数。来看下面的打招呼程序：

```
    who := "Alice "
    if len(os.Args) > 1 {
        who += strings.Join(os.Args[1:], " ")
    }
    fmt.Println("Good Morning", who)
```

我们在 IDE 或编辑器中直接运行这个程序输出：`Good Morning Alice`

我们在命令行运行`os_args or ./os_args`会得到同样的结果。

但是我们在命令行加入参数，像这样：`os_args John Bill Marc Luke`，将得到这样的输出：`Good Morning Alice John Bill Marc Luke`

这个命令行参数会放置在切片`os.Args[]`中（以空格分隔），从索引1开始（`os.Args[0]`放的是程序本身的名字，在本例中是`os_args`）。函数`strings.Join`以空格为间隔连接这些参数。

