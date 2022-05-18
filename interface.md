## interface
```golang
package _interface

import "testing"

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

=== RUN   TestClient
    interface_test.go:33: fmt.Println("hello World")
--- PASS: TestClient (0.00s)


```
- go 接口是非入侵的，实现不依赖于接口的定义
- 所以接口的定义可以包含在接口使用者包内

