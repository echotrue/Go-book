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


