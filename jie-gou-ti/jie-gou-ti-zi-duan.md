结构体字段使用`.(点好)`来访问

```
func main() {
	type people struct {
		x,y int
	}

	p := people{1,2}
	println(p.x)
	p.x = 4
	println(p.x)
}
```



