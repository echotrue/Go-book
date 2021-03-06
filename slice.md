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

#### 构造slice

slice由函数make\(\)创建.这回分配一个全是零值的数组并且返回个slice指向这个数组

```
a := make([]int,5)
a := make([]int,5,10)//第二个参数是slice的长度,第三个参数是slice的容量
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

可动态增减元素是数组切片比数组更为强大的功能.与数组相比,数组切片多了一个存储能力\(capacity\)的概念,即元素个数和分配空间可以是两个不同的值.合理的设置存储能力的值,可以大大的降低数组切片内部重新分配内存和搬送内存块的频率,从而大大提高程序性能.数组切片支持Go语言内置的`cap()`函数和`len()`函数,`cap()`函数返回的是数组切片分配的空间大小,而`len()`函数返回的是数组切片中当前所存储的元素的个数.append向slice里面追加一个或者多个元素,然后返回一个和slice一样类型的slice.

```
    aSlice := make([]int, 5, 10)
    aSlice = append(aSlice, 1,2,3)
```

> **\[info\]** 注释
>
> 以上代码向aslice里面追加3个元素,追加后aslice的长度是8

也可以将一个数组切片直接追加到另外一个数组切片末尾

```
   bSlice := []int{4, 5, 6}
    aSlice = append(aSlice, bSlice...) //等同于aSlice = append(aSlice, 4, 5, 6)
```

#### slice内容复制

```
n1 := copy(s,a[x:y])
```

copy函数用户将内容从一个数组切片复制到另外一个数组切片,被复制的是数组元素的值而不是引用,如果两个数组切片不一样大,就会你找较小的那个数组切片的元素个数进行复制,并且返回复制的元素个数

```
aSlice := []int{1, 2, 3, 4, 5}
    bSlice := []int{5, 4, 3}
    copy(bSlice, aSlice) // 只会复制aSlice的前3个元素到bSlice中
    copy(aSlice, bSlice) // 只会复制bSlice的3个元素到aSlice的前3个位置
    slice1 := copy(s, a[:])         //说明：把a 全部的值复制到s 中
    slice2 := copy(s, a[2:])        //说明：把指定范围的值复制到s 的开始位置
    slice3 := copy(s[4:6], a[6:8])  //说明：把指定范围的值复制到s 的指定位置
```



