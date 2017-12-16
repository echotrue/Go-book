#### 包的说明

每个Go程序都是由包组成的,程序运行的入口是包`main` .按照惯例,包名与导入路径的最后一个目录一致.例如`"math/rand"   包是`由`package rand`语句开始

#### 包的导入

单个包的导入

```
import "fmt"
```

多个包的导入:

```
import (
    "fmt"
    "math"
)
```

或者:

```
import "fmt"
import "math"
```

> **\[info\]注:**
>
> 导入多个包时使用打包导入语句\(即使用圆括号组合导入\)是更好的形式.

#### 包的可见性

包的可见性相当于Java中类成员的访问权限:public,private,protect.再Go语言中首字母大写的名称都可以再包外部被访问

```
fmt.Println("hello world")
```

如上:fmt包中的`Println()`方法可以被访问.

