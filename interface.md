## interface
```golang
package _interface

import (
	"fmt"
	"testing"
	"time"
)

/*
	接口的定义
	type Programmer interface{
		WriteHelloWorld() Code
	}

	接口的实现示例
	type CoProgrammer struct{}
	func (p *GoProgrammer) WriteHelloWorle() Code {
		return "fmt.Println(xxxxx)"
	}

*/

type Programmer interface {
	WriteHelloWorld() string
}

type GoProgrammer struct {
}

func (g *GoProgrammer) WriteHelloWorld() string {
	return "fmt.Println(\"hello World\")"
}

func TestClient(t *testing.T) {
	var p Programmer
	p = new(GoProgrammer)
	t.Log(p.WriteHelloWorld())
}

//接口变量
var prog Programmer = &GoProgrammer{}

//此时 prog就是接口变量 包含了 type GoProgrammer struct {}类型 和 &GoProgrammer{}的数据
// 自定义类型
type MyPoint int
type InrXonc func(op int) int
type IntConv func(op int) int

func timeSpent(inner IntConv) IntConv {
	return func(n int) int {
		start := time.Now()
		ret := inner(n)
		fmt.Println("time sopent", time.Since(start).Seconds())
		return ret
	}
}

//扩展
type Pet struct {
}

func (p *Pet) Speak() {
	fmt.Println("...")
}

func (p *Pet) SpeakTo(host string) {
	p.Speak()
	fmt.Println(" ", host)
}

type Dog struct {
	p *Pet
}

func (d *Dog) Speak() {
	//d.p.Speak()
	fmt.Println("Wakakak") //扩展重写
}

func (d *Dog) SpeakTo(dog string) {
	d.Speak() //如果上面重写了，就需要自己实现这个方法啦，不然他还是调用的p的speak()
	//d.p.SpeakTo(dog)
	fmt.Println("speak to ", dog)
}

func TestDog(t *testing.T) {
	dog := new(Dog)
	dog.SpeakTo("Chao")
}

//匿名嵌套，不需要自己写方法 ,好像获得了继承，但是这样是不可以重载父类的方法
type Cat struct {
	Pet
}

func TestCat(t *testing.T) {
	cat := new(Cat)
	cat.Speak()
	cat.SpeakTo("miaomiaomiao")
}




```
- go 接口是非入侵的，实现不依赖于接口的定义
- 所以接口的定义可以包含在接口使用者包内


## 多态
- 
## 多态
```golang
type Code string
type Programmer1 interface {
	WriteHelloWorld1() Code
}
type GoProgrammer1 struct {
}

func (p *GoProgrammer1) WriteHelloWorld1() Code {
	return "fmt.Println(\"Hello World\")"
}

type JavaProgrammer1 struct{}

func (p *JavaProgrammer1) WriteHelloWorld1() Code {
	return "System.out.Println(\"Hello World\")"
}

func writeFirsttProgram(p Programmer1) {
	fmt.Printf("%T ,%v \n", p, p.WriteHelloWorld1())
}

func TestPolymorphism(t *testing.T) {
	goProg := new(GoProgrammer1)
	javaProg := new(JavaProgrammer1)
	writeFirsttProgram(goProg)
	writeFirsttProgram(javaProg)
}



```

##空接口与断言
- 空接口可以表示任何类型
- 通过断言来将空接口转换为制定的类型
```golong

// 空接口和断言
func DoSomething(p interface{}) {
	if i, ok := p.(int); ok {
		fmt.Println("Integer", i)
		return
	}
	if s, ok := p.(string); ok {
		fmt.Println("string", s)
		return
	}

	fmt.Println("Unknow Type")
}

func TestEmptyInterFaceAssertion(t *testing.T) {
	DoSomething(10)
	DoSomething("test")
	DoSomething(2.3)
}
=== RUN   TestEmptyInterFaceAssertion
Integer 10
string test
Unknow Type
--- PASS: TestEmptyInterFaceAssertion (0.00s)

```
//接口的最佳实践
1.倾向于使用小的接口定义，很多接口只包含一个方法
2.较大的接口可以由多个小接口定义组合而成
3.只依赖于非必要功能的最小接口
