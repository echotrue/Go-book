Go具有指针.指针保存了变量的内存地址

类型\*T是指向类型T的值的指针.其零值是nil

```
func main() {
    var f *int
    fmt.Println(f)
}
```

&符号会生成一个指向其作用对象的指针.

```
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i
```



