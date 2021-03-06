而Go有一个特殊的类型，通道（channel），像是通道（管道），可以通过它们发送类型化的数据在协程之间通信，可以避开所有内存共享导致的坑；通道的通信方式保证了同步性。数据通过通道：同一时间只有一个协程可以访问数据：所以不会出现数据竞争，设计如此。数据的归属（可以读写数据的能力）被传递

通常使用这样的格式来声明通道：var identifier chan datatype

未初始化的通道的值是nil。

所以通道只能传输一种类型的数据，比如 chan int 或者 chan string，所有的类型都可以用于通道，空接口 interface{} 也可以。甚至可以（有时非常有用）创建通道的通道。

通道实际上是类型化消息的队列：使数据得以传输。它是先进先出（FIFO）的结构所以可以保证发送给他们的元素的顺序（有些人知道，通道可以比作 Unix shells 中的双向管道（two-way pipe））。通道也是引用类型，所以我们使用 make\(\) 函数来给它分配内存通

```
func main() {
    ch := make(chan string)
    go sendData(ch)
    go getData(ch)
    time.Sleep(1e9)
}

func sendData(ch chan string) {
    ch <- "1"
    ch <- "2"
    ch <- "3"
    ch <- "4"
    ch <- "5"
}

func getData(ch chan string) {
    var input string
    for {
        input = <-ch
        fmt.Println(input)
    }
}
```

##### 通道阻塞

默认情况下，通信是同步且无缓冲的：在有接受者接收数据之前，发送不会结束。可以想象一个无缓冲的通道在没有空间来保存数据的时候：必须要一个接收者准备好接收通道的数据然后发送者可以直接把数据发送给接收者。所以通道的发送/接收操作在对方准备好之前是阻塞的：

1）对于同一个通道，发送操作（协程或者函数中的），在接收者准备好之前是阻塞的：如果ch中的数据无人接收，就无法再给通道传入其他数据：新的输入无法在通道非空的情况下传入。所以发送操作会等待 ch 再次变为可用状态：就是通道值被接收时（可以传入变量）。

2）对于同一个通道，接收操作是阻塞的（协程或函数中的），直到发送者可用：如果通道中没有数据，接收者就阻塞了。

尽管这看上去是非常严格的约束，实际在大部分情况下工作的很不错。

以下例子验证了以上理论 , 一个协程在无限循环中给通道发送整数数据。不过因为没有接收者，只输出了一个数字 0

```
func main() {
    ch := make(chan int)
    go pump(ch)
    fmt.Println(<-ch)
}

func pump(ch chan int) {
    for i := 0; ; i++ {
        ch <- i
    }
}
```

为通道解除阻塞定义了`suck`函数来在无限循环中读取通道

```
func main() {
    ch := make(chan int)
    go pump(ch)
    go suck(ch)
    time.Sleep(1e9)
}

func pump(ch chan int) {
    for i := 0; ; i++ {
        ch <- i
    }
}
func suck(ch chan int) {
    for {
        fmt.Println(<-ch)
    }
}
```

##### 同步通道-使用带缓冲的通道

一个无缓冲通道只能包含 1 个元素，有时显得很局限。我们给通道提供了一个缓存，可以在扩展的`make`命令中设置它的容量，如下

```
buf := 100
ch1 := make(chan string, buf)
```

buf 是通道可以同时容纳的元素（这里是 string）个数

在缓冲满载（缓冲被全部使用）之前，给一个带缓冲的通道发送数据是不会阻塞的，而从通道读取数据也不会阻塞，直到缓冲空了。

缓冲容量和类型无关，所以可以（尽管可能导致危险）给一些通道设置不同的容量，只要他们拥有同样的元素类型。内置的`cap`函数可以返回缓冲区的容量

如果容量大于 0，通道就是异步的了：缓冲满载（发送）或变空（接收）之前通信不会阻塞，元素会按照发送的顺序被接收。如果容量是0或者未设置，通信仅在收发双方准备好的情况下才可以成功。

```
ch :=make(chan type, value)
```

* value == 0 -&gt;synchronous, unbuffered \(阻塞）

* value &gt;0 -&gt; asynchronous, buffered（非阻塞）取决于value元素

若使用通道的缓冲，你的程序会在“请求”激增的时候表现更好：更具弹性，专业术语叫：更具有伸缩性（scalable）。要在首要位置使用无缓冲通道来设计算法，只在不确定的情况下使用缓冲。

```
func main() {
    c := make(chan int, 50)
    go func() {
        time.Sleep(15 * 1e9)
        x := <-c
        fmt.Println("received", x)
    }()
    fmt.Println("sending", 10)
    c <- 10
    fmt.Println("sent", 10)
}
```

以上代码会直接输出

```
sending 10
sent 10
```

因为接下来main\(\)结束了,因为channel c的容量大于0.所以在通道缓冲满载之前,往通道发送数据是不会阻塞的.而从通道读数据也不会阻塞,知道通道空了.修改c:=make\(chan int ,50\)为c:=make\(chan int\)然后对比两次输出的结果.容量为0的时候就是同步阻塞,即发送方和接收方都准备好的情况下,直到通信完成.

