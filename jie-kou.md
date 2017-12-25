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



