结构体字段可以通过结构体指针来访问.通过指针间接的访问是透明的

```
func main() {
	type people struct {
		x,y int
	}

	p := people{1,2}
	m :=&p
	println(m.x)
	m.x=5
	println(m.x)
}
```



