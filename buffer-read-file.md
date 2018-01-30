在下面的例子中，我们结合使用了缓冲读取文件和命令行 flag 解析这两项技术。如果不加参数，那么你输入什么屏幕就打印什么。

参数被认为是文件名，如果文件存在的话就打印文件内容到屏幕

`func cat(r *bufio.Reader) {`

`    for {`

`        buf, err := r.ReadBytes('\n')`

`        if err == io.EOF {`

`            break`

`        }`

`        fmt.Fprintf(os.Stdout, "%s", buf)`

`    }`

`    return`

`}`



`func main() {`

`    flag.Parse() //解析命令行参数`

`    //flag.Args()//获取参数`

`    if flag.NArg() == 0 {`

`        cat(bufio.NewReader(os.Stdin))`

`    }`

`    for i := 0; i < flag.NArg(); i++ {`

`        f, err := os.Open(flag.Arg(i))`

`        if err != nil {`

`            fmt.Fprintf(os.Stderr, "%s:error reading from %s: %s\n", os.Args[0], flag.Arg(i), err.Error())`

`            continue`

`        }`

`        cat(bufio.NewReader(f))`

`    }`

`}`



