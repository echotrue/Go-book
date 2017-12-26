一个普遍存在的接口是`fmt`包中定义的`Stringer`。

```
type Stringer interface {
    String() string
}
```

`Stringer`是一个可以用字符串描述自己的类型。\`fmt\`包 （还有许多其他包）使用这个来进行输出。

