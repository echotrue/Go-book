#### slice 发音

\[slaɪs\]

#### slice介绍

数组是固定长度,数组的长度再被定义以后就无法修改.数组是值类型,每次传递都会产生一份副本.

一个`slice`指向一个序列的值,并且包含了长度信息.

`[]T`是一个元素类型为`T`的`slice`

`len(s)`返回`slice s`的长度

#### slice 元素可以为任何类型

以下是以一slice作为slice的元素:

```
func main() {
    a := []string{"1"}
    b:=[2]int{1,2}

    fmt.Printf("%T", a)
    fmt.Printf("%T\n", b)
    game := [][]string{
        []string{"a1", "a2", "a3"},
        []string{"b1", "b2", "b3"},
        []string{"c1", "c2", "c3"},
    }
    printSlice(game)
}

func printSlice(s [][]string) {
    for i := 0; i < len(s); i++ {
        fmt.Printf("%s\n", strings.Join(s[i], "-"))
    }
}
```

> **\[info\] 注解**
>
> game是一个slice,game的类型是\[\]\[\]string.game有三个元素,game里面元素的类型则是\[\]string



