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
