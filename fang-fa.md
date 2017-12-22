#### 方法的定义

Go语言中,类型可以定义接收此类型的函数即方法.方法就是有接收者的函数

```
type S struct { i int }
func (p *S) Get() int { return p.i }
func (p *S) Put(v int) { p.i = v }
```



