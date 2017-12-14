和for一样,if语句可以在条件之前执行一个简单的语句,由这个语句定义的变量作用域仅在if范围之内

```
package main

import "fmt"

func check(len int) int {
    if v := 1; v > len {
        return len
    }
    return 0
}
func main() {
    re := check(10)
    fmt.Println(re)
}
```

如上代码:如果将 `return 0` 替换为 `return v`会报错:`undefined: v`



```
**[terminal]
```

```
**[prompt foo@joe]**[path ~]**[delimiter  $ ]**[command ./myscript]
Normal output line. Nothing special here...
But...
You can add some colors. What about a warning message?
**[warning [WARNING] The color depends on the theme. Could look normal too]
What about an error message?
**[error [ERROR] This is not the error you are looking for]
```



