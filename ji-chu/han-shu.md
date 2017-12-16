#### 函数的基本格式

函数可以没有参数或接受多个参数,再下面的例子中`add()`方法接受两个`int`类型的参数.并且函数返回类型是整型

```
func add(x int, y int) int {
    return x + y
}
```

当两个或多个连续函数参数是同一类型,可以简写成如下形式

```
func add(x, y int) int {
    return x + y
}
```

#### 函数多值返回

Go语言中函数可以返回任意数量的返回值

```
func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```



