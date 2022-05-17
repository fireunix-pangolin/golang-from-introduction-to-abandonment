# 编写测试程序
 - 源码文件以 _test 结尾: xxx_test.go
 - 测试方法名称以Test开头(大写表示包外可访问): func TestXXX(t *testing.T) {...}
 import "testing"
```golang
package myself_test

import "testing"

func TestFirstTry(t *testing.T) {
	t.Log("Try myself test...")
}


```
 - 运行时记得是 go test xxxx_test.go 
 - go test -v fib_test.go 才能显示t.Log里面的内容    


```golang
package fibna

import (
	"testing"
)

func TestFibList(t *testing.T) {
	var a int = 1 //变量的赋值
	var b int = 1 //变量的赋值
	/*
			//也可以摘要赋值
		var (
			a int = 1
			b int = 1
		)
		//
		也有一种类型推断
		a:= 1
		b:= 1
	*/
	t.Log(a)
	for i := 0; i < 5; i++ {
		t.Log("", b)
		tmp := a
		a = b
		b = a + tmp
	}
}


```

