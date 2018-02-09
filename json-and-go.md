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

> \[info\] 注释
>
> 只有可以表示为有效JSON的数据结构才会被编码
>
> * JSON对象只支持字符串作为键;要编码一个Go map类型，它必须是map \[string\] T（其中T是json包所支持的任何Go类型）
> * Channel, complex, and function 类型不能被编码
> * 循环数据结构不支持;他们将使Marshal进入一个无限循环
> * 指针将被编码为它们指向的值（如果为null，则为nil
> * json包只能访问结构类型的输出字段（以大写字母开头）。因此，只有结构的导出字段才会出现在JSON输出中

#### 解码

使用Unmarshal

```
func Unmarshal(data []byte, v interface{}) error
```

首先需要创建一个解码数据存储的地方

```
var m Message
```



