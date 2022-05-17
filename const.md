# 常量
```
package test_const

import "testing"

const (
	Monday = iota + 1 //递增加一
	Tuesday
	Wednesday
	Thursday
	Friday
	Saturday
	Sunday
)

const (
	Open = 1 << iota //连续位常量
	Close
	Pending
)

func TestConstTest(t *testing.T) {
	a := 7 //0111
	t.Log(Open, Close, Pending, a&Open)
	t.Log(Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday)
}

```
