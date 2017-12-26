```
package main

import "fmt"

type animal interface {
    desc()
}
type people struct {
    name string
}

func (p *people) desc() {
    fmt.Printf("people's name is %s\n", p.name)
}

func (p *people) setName(name string) {
    p.name = name
}

func main() {
    man := new(people)
    man.setName("axlrose")
    //man.desc()
    desc:=animal(man)
    desc.desc()

}
```

定义了一个接口`animal`,定义了一个结构体`people`.结构体实现了结构的`desc`方法,且结构体有一个自己的方法`setName`.

#### 多态

* 多个类型可以实现同一个接口。

* 一个类型可以实现多个接口。



