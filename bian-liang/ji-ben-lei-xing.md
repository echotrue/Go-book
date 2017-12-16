Go基本类型

```
bool
string
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
byte // uint8 的别名
rune // int32 的别名
     // 代表一个Unicode码

float32 float64
complex64 complex128
```

> **\[success\]**  注释:
>
> `int`，`uint`和`uintptr`类型在32位的系统上一般是32位，而在64位系统上是64位。当你需要使用一个整数类型时，你应该首选`int`，仅当有特别的理由才使用定长整数类型或者无符号整数类型。



