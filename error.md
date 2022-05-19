# Go的错误机制
package error

//Go的错误机制
- 没有异常机制
- error类型实现了error接口
 ```golang
type error interface{
	 error() string
 }```
- 可以通过errors.New来快速创建错误实例
`errors.New("n muser be in the range [0,1]")`

```golang
package error

import (
	"errors"
	"fmt"
	"testing"
)

func GetFibonacci(n int) ([]int, error) {
	if n < 0 || n > 100 {
		return nil, errors.New("n must > 0 and < 100")
	}
	fibList := []int{1, 1}
	for i := 2; i < n; i++ {
		fibList = append(fibList, fibList[i-2]+fibList[i-1])

	}
	return fibList, nil
}

func TestFibonacci(t *testing.T) {
	if v, err := GetFibonacci(10); err != nil {
		fmt.Println("Error", err.Error())
	} else {
		fmt.Println(v)
	}
}
```
=== RUN   TestFibonacci
[1 1 2 3 5 8 13 21 34 55]
--- PASS: TestFibonacci (0.00s)
PASS

## 根据错误类型
```golang
package error

import (
	"errors"
	"fmt"
	"testing"
)

var LessThanTwoError = errors.New("error1")
var LargerThanTwoError = errors.New("error2")

func GetFibonacci(n int) ([]int, error) {
	if n < 0 {
		return nil, LessThanTwoError
	}
	if n > 100 {
		return nil, LargerThanTwoError

	}
	fibList := []int{1, 1}
	for i := 2; i < n; i++ {
		fibList = append(fibList, fibList[i-2]+fibList[i-1])

	}
	return fibList, nil
}

func TestFibonacci(t *testing.T) {
	if v, err := GetFibonacci(110); err != nil {
		if err == LessThanTwoError {
			fmt.Println("Error n < 2", err.Error())
      	return
		}
		if err == LargerThanTwoError {
			fmt.Println("Error n >100 ", err.Error())
      	return
		}
	} else {
		fmt.Println(v)
	}
}

=== RUN   TestFibonacci
Error n >100  error2
--- PASS: TestFibonacci (0.00s)
PASS


```

## 及早失败，避免嵌套
- 一般是判断有错，输出错误然后return

# panic 退出不可恢复的错误
  - panic 用于不可恢复的错误
  - panic 退出前会执行defer指定的内容
  - 和 os.Exit 退出时不会调用defer指定的函数，不输出当前调用栈信息
 ```golang
 func TestPanic(t *testing.T) {
	defer func() {
		fmt.Println("出错啦！", time.Now())
	}()

	fmt.Println("panic start")
	panic(errors.New("someting wrong!"))
}

=== RUN   TestPanic
panic start
出错啦！ 2022-05-19 14:23:37.640677 +0800 CST m=+0.001330668
--- FAIL: TestPanic (0.00s)
panic: someting wrong! [recovered]
        panic: someting wrong!

goroutine 5 [running]:
testing.tRunner.func1.2({0x1022d78e0, 0x1400004c540})
        /Users/zhanghanzhong/sdk/go1.17.7/src/testing/testing.go:1209 +0x258
testing.tRunner.func1(0x1400010e4e0)
        /Users/zhanghanzhong/sdk/go1.17.7/src/testing/testing.go:1212 +0x284
panic({0x1022d78e0, 0x1400004c540})
        /Users/zhanghanzhong/sdk/go1.17.7/src/runtime/panic.go:1038 +0x21c
command-line-arguments.TestPanic(0x1400010e4e0)
        /Users/zhanghanzhong/go_learn/src/error/error/error_test.go:55 +0xc8
testing.tRunner(0x1400010e4e0, 0x1022f1258)
        /Users/zhanghanzhong/sdk/go1.17.7/src/testing/testing.go:1259 +0xfc
created by testing.(*T).Run
        /Users/zhanghanzhong/sdk/go1.17.7/src/testing/testing.go:1306 +0x328
FAIL    command-line-arguments  0.415s
FAIL

 
 ```
 
 
 # recover
 
 ```golang
 
 func TestPanic1(t *testing.T) {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("recovered from ", err)
		}
	}()

	fmt.Println("panic start")
	panic(errors.New("someting wrong!"))
}
 
 === RUN   TestPanic1
panic start
recovered from  someting wrong!
--- PASS: TestPanic1 (0.00s)
PASS
ok      command-line-arguments  0.408s

 
 ```
 
 
