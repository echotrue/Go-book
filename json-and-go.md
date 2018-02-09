#### Json编码

使用Marshal函数来编码Json数据

```
func Marshal(v interface{}) ([]byte, error)
```

定义一个`Go`的数据结构`Message`:

```
type Message struct {
    Name string
    Body string
    Time int64
}
```

和一个`Message`的实例:

```
m := Message{"Alice", "Hello", 1294706395881547000}
```

生成`json`编码格式的数据:

```
b, err := json.Marshal(m)
```

如果一切正常,`err`为`nil`,`b`是一个包含了该`json`数据的`[]byte`

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

使用`Unmarshal`

```
func Unmarshal(data []byte, v interface{}) error
```

首先需要创建一个解码数据存储的地方

```
var m Message
```

并且调用`json.Unmarshal`方法传递一个`[]byte`类型的`json`数据b和一个指向`m`的指针

```
err:=json.Unmarshal(b,&m)
```

如果`b`包含匹配`m`的有效`json`数据。`err`的值为`nil`，来自b的数据将会被存储在结构m中，就像通过如下赋值：

```
m=Message{
    Name:"alice",
    Body:"hello",
    Time:1293484739238,
}
```

Unmarshal如何识别存储解码数据的字段，对于给定的JSON键Foo，Unmarshal将通过目标结构的字段来查找（按照优先顺序）

* 带有Foo标签的导出字段
* 名字为Foo的导出字段
* 名为FOO或者FoO的输出字段或者Foo的某些其他不区分大小写的匹配

当json数据的结构和go的类型不完全匹配时会发生什么呢？

Unmarshal将只会解析在目标类型中可以找到的字段。当你希望从大的json块中解析几个特殊的字段时，此行为非常有用。这也意味着目标结构中任何未导出的字段将不受影响

##### 但是如果你事先不知道你的json数据结构呢？

#### Generic Json with interface{}

上面是在知道json结构的情况下来解析json，如果不知道被解析的数据的结构时又该如何解析呢？

我们知道interface{}可以用来存储任意数据类型的对象，所以我们可以用interface{}来存储未知结构的json数据的解析结果

