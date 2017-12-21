#### map的初始化以及赋值

* 声明map,创建map,赋值

```
    var m map[int]string     //声明map
    m = make(map[int]string) //创建一个非nil的map,nil map不能赋值
    m[1] = "name"            //给map赋值
    m[2] = "age"
```

* 直接创建

```
m2 := make(map[string]string)
```

* 初始化并赋值

```
m3 := map[string]string{
	"a": "aa",
	"b": "bb",
}
```

#### 遍历map

```

```


