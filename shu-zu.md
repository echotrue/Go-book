#### 数组定义

`[n]T`是一个有n个类型为T的值的数组.数组的长度是其类型的一部分，因此数组不能改变大小。 这看起来是一个制约，但是请不要担心； Go 提供了更加便利的方式来使用数组。

```
var a [10]int
```

以上表达式的意思是:a是一个有10个整型元素的数组

#### 数组声明

```
var a [5]byte //长度为5的数组，每个元素为一个字节
var b [2*N] struct { x, y int5 } //复杂类型数组
var c [5]*int // 指针数组
var d [2][3]int //二维数组
var e [2][3][4]int //等同于[2]([3]([4]int))
```

#### 数组初始化

先声明再初始化

```
var a [3]string
a = {'1','2','3'}
```

直接声明并初始化

    a := [3]byte{'1', '2', '3'} //声明并初始化一个长度为3的byte数组
    a := [...]byte{'1', '2', '3'} //可以省略长度而采用`...`的方式，Go会自动根据元素个数来计算长度
    d := [2][3]int{[3]int{1,2,3},[3]int{4,5,6}}
    d := [2][3]int{{1,2,3},{4,5,6}} //如果内部的元素和外部的一样，那么上面的声明可以简化，直接忽略内部的
    类型

```
    var arr1 [5]int
    arr2 := [5]int{1, 2, 3, 4, 5}   //指定长度为5，并赋5个初始值
    arr3 := [5]int{1, 2, 3}         //指定长度为5，对前3个元素进行赋值，其他元素为零值
    arr4 := [5]int{4: 1}            //指定长度为5，对第5个元素赋值
    arr5 := [...]int{1, 2, 3, 4, 5} //不指定长度，对数组赋以5个值
    arr6 := [...]int{8: 1}          //不指定长度，对第9个元素（下标为8）赋值1
    fmt.Println(arr1, arr2, arr3, arr4, arr5, arr6)
```

#### 数组元素的访问

计算长度会用Go内置函数`leng()`

普通访问方式

```
for i := 0; i < len(arr); i++ {
    fmt.Println(i, arr[i])
}
```

通过`range`访问

```
for i, v := range arr {
    fmt.Println(i, v)
}
```

#### 数组值传递

```
func modify(arr [5]int){
   arr[0] = 10
   fmt.Println("In modify(), arr values:", arr)
}

func main(){
    arr := [5]int{1, 2, 3, 4, 5}
    modify(arr)
    fmt.Println("In main(), arr values:", arr)
}

输出结果为:
In modify(), arr values: [10 2 3 4 5]
In main(), arr values: [1 2 3 4 5]
```



