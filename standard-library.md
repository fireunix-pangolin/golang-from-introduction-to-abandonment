# Standard library

官方地址(https://pkg.go.dev/builtin#pkg-index)



[TOC]



----

## 数值类型

| 类型ch     | 类型       | 字节宽度                                                     | 说明&取值范围                                                | 相关运算符号                                                 |
| ---------- | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 布尔       | bool       | 1字节                                                        | true 非0为真  false 0                                        | 与 &&  或 \|\|  非 !                                         |
| 整型       | int        | 32位系统四字节，64位系统8字节                                | 有符号整型 2的32次方 或 64次方                               | 常用包  math  math/rand                                      |
| 整型       | uint       | 32位系统四字节，64位系统8字节                                | 无符号整型                                                   |                                                              |
| 整型       | rune       | 4字节                                                        | Unicode码，取值范围同uint32                                  |                                                              |
| 整型       | int8       | 1字节                                                        | 8为表示有符号整型 取值范围为:[-128, 127]                     |                                                              |
| 整型       | int16      | 2字节                                                        | 用 16 位表示的有符号整型，取值范围为： [-32768,32767]        |                                                              |
| 整型       | int32      | 4字节                                                        | 用 32 位表示的有符号整型，取值范围为： [-2147483648,2147483647] |                                                              |
| 整型       | int64      | 8字节                                                        | 用 64 位表示的有符号整型，取值范围为： [-9223372036854775808,9223372036854775807] |                                                              |
| 无符号整型 | uint8      | 1字节                                                        | 用8位标识的无符号整型，取值范围:[0,255]                      |                                                              |
| 无符号整型 | uint16     | 2字节                                                        | 用8位标识的无符号整型，取值范围:[0,65535]                    |                                                              |
| 无符号整型 | uint32     | 4字节                                                        | 用 32 位表示的无符号整型，取值范围为：[0, 4294967295]        |                                                              |
| 无符号整型 | uint64     | 8字节                                                        | 用 64 位表示的无符号整型，取值范围为：[0, 18446744073709551615] |                                                              |
| 指针       | uintptr    | 与平台有关，32 位  系统 4 字节，64 位 系统 8 字节            | 指针值的无符号整型                                           |                                                              |
| 浮点数     | float32    | 4字节                                                        |                                                              |                                                              |
| 浮点数     | float64    | 8字节                                                        | 表示法   f1 float32 = 3E-3  f2 float32 = 3.1415926  f3 float64 = 3.1E10 | 常用包      math/cmplx                                       |
| 复数型     | complex64  | complex 工厂函数  real 复数实部  imag 复数虚部               | 表示   var c1 complex64 = 1+2i  var c2 complex64 = complex(3,4)  fmt.Println(c1+ c2)  fmt.Println(real(c1),imag(c1)) |                                                              |
| 复数型     | complex128 |                                                              |                                                              |                                                              |
| 字符串     | string     | 可解析字符串 ""来创建，不能包含多行，支持特殊字符转义序列；  原生字符串 ``来创建，可包含多行不支持转义字符 | 特殊字符：  ⚫ [\\：反斜线](file://：反斜线)  ⚫ \'：单引号  ⚫ \"：双引号  ⚫ \a：响铃  ⚫ \b：退格  ⚫ \f：换页  ⚫ \n：换行  ⚫ \r：回车  ⚫ \t：制表符  ⚫ \v：垂直制表符  ⚫ \ooo：3 个 8 位数字给定的八进制码点的 Unicode  字符（不能超过\377）  ⚫ \uhhhh：4 个 16 位数字给定的十六进制码点的 Unicode 字符  ⚫ \Uhhhhhhhh：8 个 32 位数字给定的十六进制码点的  Unicode 字符  ⚫  \xhh：2 个 8 位数字给定的十六进制码点的 Unicode 字符 | a) 字符串连接：+   b) 关系运算符：>、>=、<、<=、==、!=   c) 赋值运算符：+=   d) 索引：s[index]，针对只包含 ascii 字符的字符串    e) 切片：s[start:end] ，针对只包含 ascii 字符的字符串  常用函数   a. len：获取字符串长度（针对只包含 ascii 字符的字符串）   b. string: 将 byte 或 rune 数组转换为字符串      常用包：  ⚫  fmt   ⚫  strings   ⚫  strconv   ⚫  unicode   ⚫  unicode/utf8   ⚫  bytes   ⚫  regex |
| 枚举       | = iota     |                                                              |                                                              | 用法高端待研究                                               |
| [指针]()   |            |                                                              |                                                              |                                                              |


- Go 不会对自动对数据类型转换，因此左、右操作数类型必须一致或某个字面量，可通过 类型名(数据)的语法将数据转换为对应类型。需要注意值截断和值溢出问题
- 不支持指针运算，string是值类型，默认是空字符串，不nil
- math库中就有了最大值 math.MaxInt64 math.MaxFloat64 math.MaxUint32  
```golang

package typt_test

import "testing"

type MyInt int64

func TestType(t *testing.T) {
	var a int32 = 1
	var b int64
	// b = a //不支持隐式转换
	// c = MyInt(a) //别名显示类型转换也不行
	b = int64(a)
	t.Log(a, b)

}


```

----



## 指针

每个变量在内存中都有对应的存储位置（内存地址）。

可以通过&运算符获取。

指针是用来存储变量地址的变量。

 

### 1.声明

 指针声明需要指定存储地址中对应数据的类型，并使用*作为类型前缀，指针变量声明后会被初始化nil，表示空指针。

```
var pointer01 *int

var pointer02 *float64

var pointer03 *string
```

![指针使用和输出](./img/pointer.png)

 

### 2.初始化

> 1). 使用 &运算符变量 初始化，&获取变量的存储位置来初始化指针变量 `poninter01 = &age`
>
> 2).使用new 函数初始化，new 函数根据数据类型申请内存空间并使用零值填充，并返回申请空间地址 `var pointernew = new(float32)`



### 3.操作

​	可通过运算符号 *指针变量名来访问或修改对应位置的数值

 

### 4.指针的指针

​	用来存储指针变量地址的变量叫做指针的指针

​	**访问指针的指针值

​	*访问第一层指针的地址



## 运算符

### 1.算术运算符

| 符号 | 意义 | 其他用法 | 特别的     |
| ---- | ---- | -------- | ---------- |
| +    | 加法 |          |            |
| -    | 减法 |          |            |
| *    | 乘法 |          |            |
| /    | 除法 |          | 0%任何数=0 |
| %    | 取模 |          | 1%8=1      |
| ++   | 自增 |          |            |
| --   | 自减 |          |            |
 
- GO  中没有前置++ 和-- 

### 2.位运算符

`对于负整数在计算机中使用补码表达(符号位+对应正整数  直接表示取反+1)。针对左右移操作数 必须为无符号整数`

| 符号 | 意义 | 用法                     | 特别的 |
| ---- | ---- | ------------------------ | ------ |
| ~    | 取反 |                          |        |
| &    | 与   | 两个位都为1时，结果才为1 |        |
| \|   | 或   | 一个为1则为1             |        |
| ^    | 异或 | 两个位不一样时，相异为1  |        |
| <<   |      |                          |        |
| >>   |      |                          |        |
| &^   |按位置零| 右边的对应位为1则结果为0，右边对应位为0则结果为原来   |        |
|      |      |                          |        |

### 3.关系运算符

| 符号 |
| ---- |
| <    |
| <=   |
| >    |
| >=   |
| ==   |
| !=   |

## 流程控制

### 1.if 条件语句

```golang
if bool {

 

} else if {

 

} else {

 

}

//if 支持变量赋值
package ifandfor

import "testing"

func TestIF(t *testing.T) {
	if a := 1 == 1; a {
		t.Log("1 = = 1")
	}
	//判断错误时用
	if v, err := someFun(); err == nil {
		t.Log("1")
	} else{
		t.Log("0")
	}
}

```

可以在 if 语句中初始化语句块内的局部变量，多个语句之间使用逗号(;)分隔

`if flag := true ; flag { }`

总结： 对于条件语句必须有 if 语句，可以有 0 个或多个 else if 语句，最多有 1 个 else 语句，语 句从上到下执行，执行第一个条件表达式为 true 的语句块并退出，否则执行 else 语句块退 出

### 2.switch-case 选择语句

根据输入条件的不同选择，不同的语句块进行执行(多分支)

- 条件表达式 不限制常量或者整数
- 单个case中可以出现多个结果选项，使用，号分割
- 与c语言相反，不需要用break退出case
- 可以不设置switch之后的条件表达式，这样逻辑就喝 多个if else的逻辑相同



```golang
//operate :="add"

switch operate :="add"; operate{

case "add":

fmt.Println()

case "no":

fmt.Println()

default:

fmt.Println()

}

// if else 类似的写法
func TestSwitch(t *testing.T) {
	var a int = 2
	switch {
	case 100 == a, a == 200:
		{
			fmt.Print("100")
		}
	case 0 <= a && a < 3:
		fmt.Print("0-3")
	case 3 <= a && a < 6:
		fmt.Print("3-6")
	default:
		fmt.Printf("%d.", a)
	}
}

```

也可以初始化子语句，多个语句之间使用逗号(;)分隔

`switch operate :="add" ;operate {}`

 默认滴 执行完case 语句后推出 如果需要继续执行下一个语句 在case末尾 用 fallthrough

总结：对于选择语句可以有 0 个或多个 case 语句，最多有 1 个 default 语句，选择条件为 true 的 case 语句块开始执行并退出，若所有条件为 false，则执行 default 语句块并退出。可 以通过 fallthrough 修改执行退出行为，继续执行下一条的 case 或 default 语句块。

### 3.循环语句

```
for i:=1;i<=100;i++{
}
break 用于跳出循环
continue用于跳过循环

类while的表示法
i:=1
for i<=1000{
	i++
}

无限循环
for{
	if bool {}
	break
}

迭代器循环
用于遍历可迭代对象中的每个元素，例如字符串，数组，切片，映射，通道等
text：="我爱中国"
for i,e := range text {

}

```

### 4.label 与 goto

可以通过 goto 语句任意跳转到当前函数指定的 label 位置,并且break和continue 也可以跳过自定label

![](D:\wsl\data\go-learning-notes\img\label-goto.png)

## 复合数据类型

| 类型  | 声明                                                   | 注意事项                                                     | 初始化                                                       | 用法                                                         | 常用函数                                                     |
| ----- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| array | `  var  name [10]string ` `var  name [10][10]string  ` | 数组声明需要指定组成元素的类型以及存储元素的数量（长度）。在数组声明后，其长度不 可修改，数组的每个元素会根据对应类型的零值对进行初始化 | a) 指定数组长度: `[length]type{v1, v2, …, vlength}  ` b) 使用初始化元素数量推到数组长度: `[…]type{v1, v2, …, vlength} `  c) 对指定位置元素进行初始化:` [length]type{im:vm, …, sin:in}   ` |       | `len()  ==  !=  array[start:end]/array[start:end:cap](end<=cap<=len)`获取数组的一部分 元素做为切片  ` for I,v := range array_name {}  ` |
| slice | ` var  name []string     `                             | 组成 指针 长度 容量  切片声明需要指定组成元素的类型，但不需要指定存储元素的数量（长度）。在切片声明后， 会被初始化为 nil，表示暂不存在的切片 | a) 使用字面量初始化:`[]type{v1,  v2, …, vn}`   b) 使用字面量初始化空切片: `[]type{}`   c) 指定长度和容量字面量初始化:`[]type{im:vm,  in:vn, ilength:vlength}`   d) 使用 make 函数初始化 `make([]type, len)/make([]type, len, cap)`，通过 make 函数创建长度为 len，容量 为 cap 的切片，len 必须小于等于 cap   e) 使用数组切片操作初始化：`array[start:end]/array[start:end:cap](end<=cap<=len)  ` | a) 获取切片长度和容量 使用 len 函数可获取切片的长度，使用 cap 函数可获取切片容量  ![var.go  Il External Libre  Scratches anc  13  Sfunc main() {  students  frit . , cap(students))  • fmt.Printf( format: "%q" , students)  main0  go build var.go  go setup calls>  Process finished with the exit code G ](data:image/png;base64,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)     b)切片: slice[start:end]用于创建一个新的切片，end <= src_cap 切片共享底层数组，若某个切片元素发生变化，则数组和其他有共享元素的切片也会发生变化  `slice[start:end:cap]`可用于限制新切片的容量值, end<=cap<= src_cap | `new_slice  = append(old_slice,new_eum1,new_eum2)`  使用 append 对切片增加一个或多个元素并返回修改后切片，当长度在容量范围内时只  增加长度，容量和底层数组不变。当长度超过容量范围则会创建一个新的底层数组并对 容量进行智能运算(元素数量1024 时约按原容量 0.25 倍增加)     copy(new1,  old1)  //1.不同类型的切片无法复制     //2.如果s1的长度大于s2的长度，将s2中对应位置上的值替换s1中对应位置的值     //3.如果s1的长度小于s2的长度，多余的将不做替换  来自 <https://www.cnblogs.com/yang-2018/p/11115106.html>      sort() |
| map   | ` var  test map[string]string`                         | map 声明需要指定组成元素 key 和 value 的类型，在声明后，会被初始化为 nil，表示  暂不存在的映射 | a) 使用字面量初始化:`map[ktype]vtype{k1:v1, k2:v2, …, kn:vn} `b) 使用字面量初始化空映射:`map[ktype]vtype{ }` c) 使用  make 函数初始化 `make(map[ktype]vtype)`，通过 make 函数创建映射 | 和字典一样访问                                               | `len()  k,v := students["test"] `存在 v为true 否则为false  `delete(test_map,3)  ` |
|       |                                                        |                                                              |                                                              |                                                              |                                                              |

### 数组
```golang

package array

import "testing"

func TestArray(t *testing.T) {
	//数组的声明
	var a [3]int //声明斌初始化默认值,默认初始化为0值
	a[0] = 1
	b := [3]int{1, 2, 3}
	c := [2][2]int{{2, 3}, {4, 5}}
	d := [...]int{1, 2, 3, 4, 5, 56}
	t.Log("a:", a, "b:", b, "c:", c, "d:", d)
	//遍历数组 法1
	for i := 0; i < len(d); i++ {
		t.Log("d: for :", d[i])
	}
	//遍历数组 法2
	for idx, e := range d {
		t.Log(idx, e)
	}
}


```
