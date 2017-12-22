#### 方法的定义

Go语言中,类型可以定义接收此类型的函数即方法.方法就是有接收者的函数

```
type S struct { i int }
func (p *S) Get() int { return p.i }
func (p *S) Put(v int) { p.i = v }
```

可以在除了非本地类型（包括内建类型，比如 int）的任意类型上定义方法。然而可以为内建类型定义别名，然后就可以为别名定义方法。如

