一个普遍存在的接口是`fmt`包中定义的`Stringer`。

```
type Stringer interface {
    String() string
}
```

`Stringer`是一个可以用字符串描述自己的类型。\`fmt\`包 （还有许多其他包）使用这个来进行输出。

```
type IPAddr [4]byte

func (p IPAddr) String() string {
	return fmt.Sprintf("%d.%d.%d.%d", p[0], p[1], p[2], p[3])
}

func main() {
	addrs := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for n, a := range addrs {
		fmt.Printf("%v: %v\n", n, a)
	}
}
```



