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
> dog 接口implement了name和sex接口,所以dog接口拥有了name和sex接口所有的方法.类型desc实现了dog接口.另外还有一个自己的方法descDog

#### 例:

```
package main

import "fmt"

type USB interface {
    Name() string
    Connect()
}

type PhoneConnect struct {
    name string
}

func (pc PhoneConnect) Name() string {
    return pc.name
}

func (pc PhoneConnect) Connect() {
    fmt.Println("Connect:", pc.name)
}

func main() {
    b := PhoneConnect{"phone"}
    b.Connect()
}
```

#### 接口嵌套

```
type USB interface {
    Name() string
    Connect
}
type Connect interface {
    Connect()
}
```

#### 判断结构是否实现了接口

```
//修改以上代码
func main() {
    b := PhoneConnect{"phone"}
    b.Connect()
    Disconnect(b)
}

//判断PhoneConnect 是否实现了USB借口
func Disconnect(usb USB) {
    fmt.Println("Disconnect;")
}
```

#### 类型断言

```
func Disconnect(usb USB) {
    if pc, ok := usb.(PhoneConnect); ok {
        fmt.Println("Disconnect:", pc.name)
        return
    }
    fmt.Println("Unknow device")
}
```

#### 空接口

Go语言中所有的类型都实现了空接口

```
func Disconnect(usb interface{}) {
    switch v := usb.(type) {
    case PhoneConnect:
        fmt.Println("Disconnect:", v.name)
    default:
        fmt.Println("Unknow device")
    }
}
```

`Disconnect`方法传入了一个空接口类型的参数`usb`,然后通过type switch判断参数usb的类型.从而做不同的处理

#### 接口类型转换

```
type USB interface {
    Name() string
    Connect
}
type Connect interface {
    Connect()
}
```

再接口嵌套例子中:USB可以转换为Connect类型,但是Connect类型不能转换为USB类型

```
func main() {
	b := PhoneConnect{"phone"}
	var a Connect
	a=Connect(b)
	a.Connect()
}
```



