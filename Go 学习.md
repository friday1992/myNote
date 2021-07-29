# Go 学习

> Go 安装

```
1.windoes
直接下载安装包安装
```

```
2.linux
tar -C /usr/local -xzf go1.4.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin

```

```
基本设置
go version
go env
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
安装goimports 
go get golang.org/x/tools/cmd/goimports

```

> 开发工具的使用

```
idea 
在plugin 市场中安装 go 和 filewachers
在  filewathcers中 加入goimports
vs
安装插件go
```

> 数据类型

| 序号 | 类型和描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **uint8** 无符号 8 位整型 (0 到 255)                         |
| 2    | **uint16** 无符号 16 位整型 (0 到 65535)                     |
| 3    | **uint32** 无符号 32 位整型 (0 到 4294967295)                |
| 4    | **uint64** 无符号 64 位整型 (0 到 18446744073709551615)      |
| 5    | **int8** 有符号 8 位整型 (-128 到 127)                       |
| 6    | **int16** 有符号 16 位整型 (-32768 到 32767)                 |
| 7    | **int32** 有符号 32 位整型 (-2147483648 到 2147483647)       |
| 8    | **int64** 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |

| 序号 | 类型和描述                        |
| :--- | :-------------------------------- |
| 1    | **float32** IEEE-754 32位浮点型数 |
| 2    | **float64** IEEE-754 64位浮点型数 |
| 3    | **complex64** 32 位实数和虚数     |
| 4    | **complex128** 64 位实数和虚数    |

| 序号 | 类型和描述                               |
| :--- | :--------------------------------------- |
| 1    | **byte** 类似 uint8                      |
| 2    | **rune** 类似 int32                      |
| 3    | **uint** 32 或 64 位                     |
| 4    | **int** 与 uint 一样大小                 |
| 5    | **uintptr** 无符号整型，用于存放一个指针 |

> 数组

```
	var a1 [5]int
	a2 := [5]int{1, 2, 3, 4, 5}
	a3 := [...]int{1, 2, 3}
	var grid [3][3]bool
		for i, val := range a1 {

		fmt.Printf("%d+%d\n", i, val)
	}
```

> 切片

```

```

> 构造函数

```
type treeNode struct {
	value       int
	right, left *treeNode
}

func (node treeNode) print() int {
	return node.value
}
func (node *treeNode) setValue(a int) {
	node.value = a

```

> 封装

```
首字母大写 public
首字母小写 private
为结构体定义的方法可以放在一个包内，可以是不同文件

扩展已有类型
1.封装
2.使用别名
3.内嵌
```

> go mod 的使用

> 接口

```
package main

import "fmt"

type Checken struct {

}
func (c Checken) isCheck() bool {
	fmt.Println("我是小鸡")
	return true
}
func (c Checken) Quack(){
	fmt.Println("我学鸭子叫")
}
func (c Checken) DuckGo(){
	fmt.Println("我学鸭子走路")
}
func DoDcuk(d Duck){
	d.DuckGo()
	d.Quack()
}
func main(){
	c:=Checken{}
	c.isCheck()
	DoDcuk(c)

}
```

```
package main

type Duck interface {
	Quack()
	DuckGo()
}

```

