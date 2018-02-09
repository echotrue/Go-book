#### Json编码

使用Marshal函数来编码Json数据

```
func Marshal(v interface{}) ([]byte, error)
```

定义一个Go的数据结构Message:

```
type Message struct {
    Name string
    Body string
    Time int64
}
```



