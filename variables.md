# 变量的赋值
 - 赋值可以进行自动类型推断
 - 一个赋值语句中可以对多个变量进行同时赋值

```golang
package variables

import "testing"

func TestExchange(t *testing.T) {
	a := 1
	b := 2
	/*tmp := a
	a = b
	b = tmp*/
	a, b = b, a
	t.Log(a, b)
}

```

 go test -v variables_test.go 
