os 包中有一个 string 类型的切片变量`os.Args`，用来处理一些基本的命令行参数，它在程序启动后读取命令行输入的参数。来看下面的打招呼程序：

```
	who := "Alice "
	if len(os.Args) > 1 {
		who += strings.Join(os.Args[1:], " ")
	}
	fmt.Println("Good Morning", who)
```



