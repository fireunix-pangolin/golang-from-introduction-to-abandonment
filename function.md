## 函数 func

- 函数是一等公民
- 可以有多个返回值
- 所有的参数都是值传递： slice，map，channel会有传引用的错觉
- 函数可以作为变量的值
- 函数可以作为参数的返回值

```golang


package function

import (
	"fmt"
	"math/rand"
	"testing"
	"time"
)

func returnMultiValues() (int, int) {
	return rand.Intn(10), rand.Intn(20)
}

func timeSpent(inner func(opt int) int) func(opt int) int {
	return func(n int) int {
		start := time.Now()
		ret := inner(n)
		fmt.Println("timeSpent: ", time.Since(start).Seconds())
		return ret
	}
}

func slowFunc(op int) int {
	time.Sleep(time.Second * 10)
	return op
}

func TestFun(t *testing.T) {
	a, _ := returnMultiValues()
	t.Log(a)
	tsSf := timeSpent(slowFunc)
	t.Log(tsSf(2))
}


=== RUN   TestFun
    function_test.go:30: 1
timeSpent:  10.001096167
    function_test.go:32: 2
--- PASS: TestFun (10.00s)


```

## 可变参数
```golang

package function

import "testing"

func sum(ops ...int) int {
	ret := 0
	for _, op := range ops {
		ret += op
	}
	return ret
}

func TestVarParam(t *testing.T) {
	t.Log(sum(1, 2, 3, 4, 5, 6, 7))
}
=== RUN   TestVarParam
    kebiancanshu_test.go:14: 28
--- PASS: TestVarParam (0.00s)
PASS


```

## defer延迟函数执行
- 清理资源，释放锁
```golang

package function

import (
	"fmt"
	"testing"
)

//panic 中断，即使中断了 defer仍然会执行

func Clear() {
	fmt.Println("Clear resourcdes.")
}

func TestDefer(t *testing.T) {
	defer Clear()
	fmt.Println("Starting")
}

=== RUN   TestDefer
Starting
Clear resourcdes.
--- PASS: TestDefer (0.00s)


```
