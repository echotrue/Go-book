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

#### 接口嵌套

```
package main

import "fmt"

type name interface {
    setName(name string)
}
type sex interface {
    setSex(sex string)
}
type dog interface {
    name
    sex
    descDog()
}

type desc struct {
    name string
    sex  string
}

func (description *desc) setSex(sex string) {
    description.sex = sex
}
func (description *desc) setName(name string) {
    description.name = name
}
func (description *desc) descDog() {
    fmt.Printf("%s's sex is %s\n", description.name, description.sex)
}
func main() {
    var dogs dog
    dogs = new(desc)
    dogs.setSex("女")
    dogs.setName("bly")
    dogs.descDog()
}
```

> **\[info\] **
>
> dog 接口implement了name和sex接口,所以dog接口拥有了name和sex接口所有的方法.类型desc实现了dog接口.



