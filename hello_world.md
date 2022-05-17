# 第一个go程序

```golang
//#应用程序入口
package main //程序入口必须package是main,目录名称不用。

import (
	"fmt"
	"os"
)

func main() { //程序入口必须是main函数,并且不支持传入参数
	// fmt.Println(os.Args) //使用os的函数获取参数
	if len(os.Args) > 1 {
		fmt.Println(os.Args, "wakakaka")
	}
	fmt.Println("wakakaka")
	os.Exit(0) //退出时显示指定，main函数不允许有返回值

}

```
