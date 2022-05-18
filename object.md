## Object

``` golang

package object

import (
	"fmt"
	"testing"
)

// 面向对象编程 : go不支持继承
// 封装
type Employee struct {
	Id   string
	Name string //注意没有，
	Age  int
}

func TestObject(t *testing.T) {
	//实例的创建及其初始化
	e := Employee{"0", "zhangsan", 20}
	e1 := Employee{Name: "lisi", Age: 40}
	t.Log(e, e1)
	e2 := new(Employee) //注意这里返回的引用/指针，相当于e:=&Employee{}
	e2.Id = "2"         //与其他编程语言不同的是：不需要通过实例的指针 -> 访问实例的成员
	e2.Age = 22
	e2.Name = "wangwu"

}

//行为（方法） 的定义

//在实例对应方法被调用时，实例成员会进行值复制
func (e Employee) String() string {
	return fmt.Sprintf("ID:%s-Name:%s-Age:%d", e.Id, e.Name, e.Age)
}

//通常情况下 我们使用避免内存拷贝的方法
//func (e *Employee) String() string {
//	return fmt.Sprintf("ID:%s-Name:%s-Age:%d", e.Id, e.Name, e.Age)
//}

func TestStructOperations(t *testing.T) {
	e := Employee{"0", "Bob", 20}
	t.Log(e.String())
}

=== RUN   TestObject
    object_test.go:20: ID:0-Name:zhangsan-Age:20 ID:-Name:lisi-Age:40
--- PASS: TestObject (0.00s)
=== RUN   TestStructOperations
    object_test.go:42: ID:0-Name:Bob-Age:20
--- PASS: TestStructOperations (0.00s)
```

- 俩种方法的实现区别在于是否进行了一次结构体 {"0", "Bob", 20} 的拷贝，所以推荐使用避免代表内存拷贝。
