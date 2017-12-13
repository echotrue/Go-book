和for一样,if语句可以在条件之前执行一个简单的语句,由这个语句定义的变量作用域仅在if范围之内

```
package main

import "fmt"

func check(len int) bool {
    if v := 1; v > len {
        return true
    }
    return false
}
func main() {
    re := check(10)
    fmt.Println(re)
}
```

如上代码:如果将 return false 替换为 return v

