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



