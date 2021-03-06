#### 方法的定义

Go语言中,类型可以定义接收此类型的函数即方法.方法就是有接收者的函数

```
type S struct { i int }
func (p *S) Get() int { return p.i }
func (p *S) Put(v int) { p.i = v }
```

可以在除了非本地类型（包括内建类型，比如 int）的任意类型上定义方法。然而可以为内建类型定义别名，然后就可以为别名定义方法。如

```
type Foo int // 为 int 定义别名 Foo
func (self Foo) Emit() {
    fmt.Printf("%v", self)
}
```

#### 接收者

接收者有两种，一种是值接收者，一种是指针接收者。顾名思义，值接收者，是接收者的类型是一个值，是一个副本，方法内部无法对其真正的接收者做更改；指针接收者，接收者的类型是一个指针，是接收者的引用，对这个引用的修改之间影响真正的接收者。像上面一样定义方法，将`user`改成`*user`就是指针接收者。

```
type People struct {
    name string
    age  int
}

func (p1 *People) editName(newName string) {
    p1.name = newName
}

func (p2 People) updateName(newName string) {
    p2.name = newName
}

func main() {
    //out name
    p1 := &People{"jack", 23}
    p1.editName("axlrose")
    fmt.Println(p1.name)
    p2 := People{"jack", 23}
    p2.updateName("axlrose")
    fmt.Println(p2.name)
}
```



