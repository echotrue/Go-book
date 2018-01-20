我们如何读取用户的键盘（控制台）输入呢？从键盘和标准输入`os.Stdin`读取输入，最简单的办法是使用`fmt`

包提供的 Scan 和 Sscan 开头的函数。请看以下程序：

#### Scan

Scan 扫描从标准输入中读取的文本，并将连续由空格分隔的值存储为连续的实参。 换行符计为空格。它返回成功扫描的条目数。若它少于实参数，err 就会报告原因。

```
var name string
var age int
fmt.Println("Please enter your name and age: ")
paramsCount, err := fmt.Scan(&name, &age)
if err == nil {
    fmt.Printf("%s's age is %d!(%d 个参数)\n", name, age, paramsCount)
} else {
    fmt.Println(err)
}
```

在终端使用`ctrl+d`终止输入.以上代码只输入一个参数时终止输入err就会报告原因

#### Scanf

Scanf 扫描从标准输入中读取的文本，并将连续由空格分隔的值存储为连续的实参， 其格式由 format 决定。它返回成功扫描的条目数。若返回的条目数小于实参数， 则会报告错误原因 err。

```
    var name string
    var age int
    fmt.Println("Please enter your name and age: ")
    paramsCount, err := fmt.Scanf("%s %d", &name, &age)
    if err == nil {
        fmt.Printf("%s's age is %d!(%d 个参数)\n", name, age, paramsCount)
    } else {
        fmt.Println("错误:")
        fmt.Println(err)
    }
```

#### Scanln

Scanln 类似于 Scan，但它在换行符处\(只要一碰到回车就停止扫描\)停止扫描，且最后的条目之后必须为换行符或 EOF。

#### Sscan

Sscan 扫描实参 string，并将连续由空格分隔的值存储为连续的实参。 换行符计为空格。它返回成功扫描的条目数。若它少于实参数，err 就会报告原因。

```
paramsCount, err := fmt.Sscan("axlrose 10 男", &name, &age)
```

#### Sscanf

Scanf 扫描实参 string，并将连续由空格分隔的值存储为连续的实参， 其格式由 format 决定。它返回成功解析的条目数。

```
paramsCount, err := fmt.Sscanf("axlrose 10", "%s %d", &name, &age)
```

#### Sscanln

Sscanln 类似于 Sscan，但它在换行符处停止扫描，且最后的条目之后必须为换行符或 EOF。



