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

#### slice初始化

* 直接初始化

```
name := {"axlrose","slash","bobo"}
```

* 通过数组初始化

```
    arr := [5]int{1, 2, 3, 4, 5}
    var aSlice, bSlice, cSlice, dSlice []int
    aSlice = arr[1:3]//[2 3]
    bSlice = arr[:3]//[1 2 3]
    cSlice = arr[3:]//[4 5]
    dSlice = arr[:]//[1 2 3 4 5]
    fmt.Println(aSlice)
    fmt.Println(bSlice)
    fmt.Println(cSlice)
    fmt.Println(dSlice)
```

> **\[info\] 注释:**
>
> slice通过array\[begin,end\]来去获取.begin是开始的位置,end是结束的位置但是不包含end
>
> begin默认是0,end默认是数组的长度\(len\(arr\)\).

* 通过slice初始化\(同数组\)

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

#### slice元素遍历\(range\)

```
func main() {
    aSlice := []int{1, 2, 3, 4, 5, 6}
    for k, v := range aSlice {
        fmt.Printf("k=%d,v=%d\n", k, v)
    }
}
```

#### slice元素动态增加\(append\)

可动态增减元素是数组切片比数组更为强大的功能.与数组相比,数组切片多了一个存储能力\(capacity\)的概念,即元素个数和分配空间可以是两个不同的值.合理的设置存储能力的值,可以大大的降低数组切片内部重新分配内存和搬送内存块的频率,从而大大提高程序性能.数组切片支持Go语言内置的`cap()`函数和`len()`函数,`cap()`函数返回的是数组切片分配的空间大小,而`len()`函数返回的是数组切片中当前所存储的元素的个数.append向slice里面追加一个或者多个元素,然后返回一个和slice一样类型的slice



