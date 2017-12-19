#### slice 发音

\[slaɪs\]

#### slice介绍

* 数组是固定长度,数组的长度再被定义以后就无法修改.数组是值类型,每次传递都会产生一份副本.

* 在实际开发中数组并不能完全满足我们的需求,通常再声明数组的时候我们并不知道需要多大的数组,我们需要一个"动态长度的数组".在Go中这种数据结构叫`slice.`

* slice  并不是真正意义上的动态数组,而是一个引用类型,它总是指向一个底层的array.

* slice 也并不是一个指向数组的指针,因为它有自己的数据结构

#### slice声明

```
var aSlice []int//声明一个元素类型为int的slice
var bSlice,cSlice []string//
```

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



