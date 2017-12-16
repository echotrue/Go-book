Go具有指针.指针保存了变量的内存地址

类型\*T是指向类型T的值的指针.其零值是nil

```
func main() {
	var f *int
	fmt.Println(f)
}
```



