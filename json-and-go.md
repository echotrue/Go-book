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

我们知道interface{}可以用来存储任意数据类型的对象，所以我们可以用interface{}来存储未知结构的json数据的解析结果，JSON包中采用map\[string\]interface{}和\[\]interface{}结构来存储任意的JSON对象和数组。Go类型和JSON类型的对应关系如下

* bool 代表 JSON booleans,
* float64 代表 JSON numbers,
* string 代表 JSON strings
* nil 代表 JSON null.

现在假设我们有如下json数据：

    b := []byte(`{"Name":"Wednesday","Age":6,"Parents":["Gomez","Morticia"]}`)

如果我们在不知道它的结构的情况下，可以吧它解析到interface{}里面

```
var f interface{}
err := json.Unmarshal(b, &f)
```

这个时候f里面存储了一个map结构。他们的key是string，值存储在空的interface{}里面

```
f = map[string]interface{}{
    "Name": "Wednesday",
    "Age":  6,
    "Parents": []interface{}{
        "Gomez",
        "Morticia",
    },
}
```

那么如何来访问这些数据呢?通过断言的方式：

```
m := f.(map[string]interface{})
```

通过断言之后，你就可以通过如下方式来访问里面的数据了

```
for k, v := range m {
```

```
    switch vv := v.(type) {
    case string:
        fmt.Println(k, "is string", vv)
    case int:
        fmt.Println(k, "is int", vv)
    case float64:
        fmt.Println(k,"is float64",vv)
    case []interface{}:
        fmt.Println(k, "is an array:")
        for i, u := range vv {
            fmt.Println(i, u)
        }
    default:
        fmt.Println(k, "is of a type I don't know how to handle")
    }
}
```

### 编码和解码流

json 包提供 Decoder 和 Encoder 类型来支持常用 JSON 数据流读写。NewDecoder 和 NewEncoder 函数分别封装了 io.Reader 和 io.Writer 接口。

```
func NewDecoder(r io.Reader) *Decoder
func NewEncoder(w io.Writer) *Encoder
```

要想把 JSON 直接写入文件，可以使用 json.NewEncoder 初始化文件（或者任何实现 io.Writer 的类型），并调用 Encode\(\)；反过来与其对应的是使用 json.Decoder 和 Decode\(\) 函数：

```
func NewDecoder(r io.Reader) *Decoder
func (dec *Decoder) Decode(v interface{}) error
```

来看下接口是如何对实现进行抽象的：数据结构可以是任何类型，只要其实现了某种接口，目标或源数据要能够被编码就必须实现 io.Writer 或 io.Reader 接口。由于 Go 语言中到处都实现了 Reader 和 Writer，因此 Encoder 和 Decoder 可被应用的场景非常广泛，例如读取或写入 HTTP 连接、websockets 或文件。

#### 将json数据写入json文件

```
type Address struct {
	Type    string
	City    string
	Country string
}

type VCard struct {
	FirstName string
	LastName  string
	Addresses []*Address
	Remark    string
}

func main() {
	pa := &Address{"private", "Aartselaar", "Belgium"}
	wa := &Address{"work", "Boom", "Belgium"}
	vc := VCard{"Jan", "Kersschot", []*Address{pa, wa}, "none"}
	// fmt.Printf("%v: \n", vc) // {Jan Kersschot [0x126d2b80 0x126d2be0] none}:
	// JSON format:
	js, _ := json.Marshal(vc)
	fmt.Printf("JSON format: %s", js)
	// using an encoder:
	file, _ := os.OpenFile("vcard.json", os.O_CREATE|os.O_WRONLY, 0666)
	defer file.Close()
	enc := json.NewEncoder(file)
	err := enc.Encode(vc)
	if err != nil {
		log.Println("Error in encoding json")
	}
}
```



