## map 与工厂模式
```golang

func TestMapFunc(t *testing.T) {
	//MAP 的value 可以是一个方法 ,与接口结合是一种工厂模式
	m := map[int]func(op int) int{}
	m[1] = func(op int) int { return op }
	m[2] = func(op int) int { return op * op }
	m[3] = func(op int) int { return op * op * op }
	t.Log(m[1](2), m[2](2), m[3](2))
}

=== RUN   TestMapFunc
    map_func_test.go:11: 2 4 8
--- PASS: TestMapFunc (0.00s)


```

## set
- golang 中没有set 但是可以使用map来确定
```golang
package map_test

import "testing"

func TestSet(t *testing.T) {
	//go内置的集合中没有set实现，可以map[type] bool 来实现
	//1.元素的唯一性，2.基本操作：a.添加元素，b.判断元素是否存在，c.删除元素，d.元素个数
	mySet := map[int]bool{}
	mySet[1] = true //添加元素
	n := 2
	if mySet[n] {
		t.Log(n, "is existing")
	} else {
		t.Log(n, "is not exist")
	}

	t.Log(len(mySet)) //元素个数

	delete(mySet, 1) //删除元素
}


=== RUN   TestSet
    set_test.go:14: 2 is not exist
    set_test.go:17: 1
--- PASS: TestSet (0.00s)

```

## string
- string 是数据类型，不是引用或指针类型
- string 是**只读**的byte slice,len函数可以获取包含 byte的个数，和实际有多少字符不一样
- string 的 byte数组可以存放任何数据

```golang
package string

import "testing"

func TestString(t *testing.T) {
	//string 是数据类型，不是引用或指针类型
	//string 是只读的byte slice,len函数可以获取包含 byte的数 ,和实际有多少字符不一样
	//string 的 byte数组可以存放任何数据
	var s string
	s = "hello"
	t.Log(s, len(s))
	s1 := "\xE4\xB8\xA5"
	t.Log(s1, len(s1))
}

=== RUN   TestString
    string_test.go:11: hello 5
    string_test.go:13: 严 3
--- PASS: TestString (0.00s)
PASS



```
