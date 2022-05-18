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

// 字符串常用函数 俩个常用的package  strings 和 strconv
func TestString(t *testing.T) {
	//string 是数据类型，不是引用或指针类型
	//string 是**只读**的byte slice,len函数可以获取包含 byte的数 ,和实际有多少字符不一样
	//string 的 byte数组可以存放任何数据
	var s string
	s = "hello"
	t.Log(s, len(s))
	s1 := "\xE4\xB8\xA5"
	t.Log(s1, len(s1))

	s3 := "中"
	t.Log(len(s3))  //是byte数
	c := []rune(s3) //获取unicode
	t.Log("中 unicode 是一种字符集 ：", c[0], "中 utf8 转换为字节序列的规则 ：", s3)
	t.Logf("中 unicode 是一种字符集 %x ; 中 utf8 转换为字节序列的规则 ： %x", c[0], s3)

	//遍历字符串
	s4 := "每天好好上班，努力学习"
	for _, v := range s4 {
		t.Logf("%[1]c,%[1]d", v)
	}
}


=== RUN   TestString
    string_test.go:12: hello 5
    string_test.go:14: 严 3
    string_test.go:17: 3
    string_test.go:19: 中 unicode 是一种字符集 ： 20013 中 utf8 转换为字节序列的规则 ： 中
    string_test.go:20: 中 unicode 是一种字符集 4e2d ; 中 utf8 转换为字节序列的规则 ： e4b8ad
    string_test.go:25: 每,27599
    string_test.go:25: 天,22825
    string_test.go:25: 好,22909
    string_test.go:25: 好,22909
    string_test.go:25: 上,19978
    string_test.go:25: 班,29677
    string_test.go:25: ，,65292
    string_test.go:25: 努,21162
    string_test.go:25: 力,21147
    string_test.go:25: 学,23398
    string_test.go:25: 习,20064
--- PASS: TestString (0.00s)
PASS



```


## sting 常用函数

```golang

package string

import (
	"strconv"
	"strings"
	"testing"
)

func TestStringFn(t *testing.T) {
	s := "A,B,C"
	parts := strings.Split(s, ",") //字符串分割
	for _, v := range parts {
		t.Log(v)
	}
	t.Log(strings.Join(parts, "+")) //字符串的连接
}

func TestStringtoInt(t *testing.T) {
	s := strconv.Itoa(10) //int转string
	t.Log("str" + s)
	if i, err := strconv.Atoi("20"); err == nil { //string转int
		t.Log(10 + i)
	} else {
		t.Log("转化错误")
	}

}


=== RUN   TestStringFn
    string_func_test.go:13: A
    string_func_test.go:13: B
    string_func_test.go:13: C
    string_func_test.go:15: A+B+C
--- PASS: TestStringFn (0.00s)
=== RUN   TestStringtoInt
    string_func_test.go:20: str10
    string_func_test.go:22: 30
--- PASS: TestStringtoInt (0.00s)
PASS


```

