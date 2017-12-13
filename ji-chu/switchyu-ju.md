Go的switch语句中的每个case最后都自带break , 当匹配成功以后就会跳出分支.

```
package main

import (
    "fmt"
    "runtime"
)

func main() {
    fmt.Print("Go runs on ")
    switch os := runtime.GOOS; os {
    case "darwin":
        fmt.Println("OS X.")
    case "linux":
        fmt.Println("Linux.")
    default:
        // freebsd, openbsd,
        // plan9, windows...
        fmt.Printf("%s.", os)
    }
}
```

如果用`fallthrough` 结束则会直接运行【紧跟的后一个】case或default语句， 不论条件是否满足都会执行

