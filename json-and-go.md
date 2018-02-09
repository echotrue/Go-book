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

和一个Message的实例:

```
m := Message{"Alice", "Hello", 1294706395881547000}
```

生成json编码格式的数据:

```
b, err := json.Marshal(m)
```

如果一切正常,err为nil,b是一个包含了该json数据的\[\]byte

    b == []byte(`{"Name":"Alice","Body":"Hello","Time":1294706395881547000}`)

> 只有可以表示为有效JSON的数据结构才会被编码



