函数也是值。他们可以像其他值一样传递，比如，函数值可以作为函数的参数或者返回值。例如:

```
func compute(fn func(float64, float64) float64) float64 {
    return fn(3, 4)
}

func main() {
    hypot := func(x, y float64) float64 {
        return math.Sqrt(x*x + y*y)
    }
    fmt.Println(hypot(5, 12))

    fmt.Println(compute(hypot))
    fmt.Println(compute(math.Pow))
}

```

* compute函数的返回值类型是float64,compute函数有一个参数fn,这个参数fn的类型是一个有两个float64类型参数且返回值也是float64类型的函数.
* math.Sqrt 是开方
* math.Pow\(x,y\)是求x的y次方



