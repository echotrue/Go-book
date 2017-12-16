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

#### 命名返回值

Go的返回值可以被命名,并且就像再函数体开头生命的变量那样使用.返回值的名称应当具有一定的意义,可以作为文档使用.没有参数的           

`return`语句返回各个返回变量的当前值,这种用法被称为裸返回.直接返回语句仅应当用在像下面这样的短函数中,再长的函数中他们会影响代码的可读性

```
func split(sum int) (x,y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}
```



