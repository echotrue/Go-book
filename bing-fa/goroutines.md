在Go语言中，每一个并发的执行单元叫作一个goroutine.设想这里的一个程序有两个函数，一个函数做计算，另一个输出结果，假设两个函数没有相互之间的调用关系。一个线性的程序会先调用其中的一个函数，然后再调用另一个。如果程序中包含多个goroutine，对两个函数的调用则可能发生在同一时刻。马上就会看到这样的一个程序。

如果你使用过操作系统或者其它语言提供的线程，那么你可以简单地把goroutine类比作一个线程，这样你就可以写出一些正确的程序了。goroutine和线程的本质区别会在9.8节中讲。

当一个程序启动时，其主函数即在一个单独的goroutine中运行，我们叫它main goroutine。新的goroutine会用go语句来创建。在语法上，go语句是一个普通的函数或方法调用前加上关键字go。go语句会使其语句中的函数在一个新创建的goroutine中运行。而go语句本身会迅速地完成。

    func main() {
        go spinner(100 * time.Millisecond)//routines
        const n = 45
        fibN := fib(n) // slow
        fmt.Printf("\rFibonacci(%d) = %d\n", n, fibN)
    }

    func spinner(delay time.Duration) {
        for {
            for _, r := range `-\|/` {
                fmt.Printf("\r%c", r)
                time.Sleep(delay)
            }
        }
    }

    func fib(x int) int {
        if x < 2 {
            return x
        }
        return fib(x-1) + fib(x-2)
    }



