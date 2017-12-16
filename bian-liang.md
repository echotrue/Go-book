#### 变量的声明

`var`语句定义了一个变量的列表.类型在后面.`var`语句可以定义再包或者函数级别

```
var c, python, java bool

func main() {
    var i int
    fmt.Println(i, c, python, java)
}
```

#### 变量的初始化

变量定义可以包含初始值,每个变量对应一个.如果初始化是使用表达式,则可以省略类型.变量会从初始值中获得类型

```
var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```



