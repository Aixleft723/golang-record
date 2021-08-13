###### golang 基础知识

1.结构体

结构体在golang里面的作用很大

- 既是数据类型
- 也拥有方法

作用相当于其他语言的类（class）

- 声明

```go
package main

import (
	"fmt"
)

type MyExample struct {
	Name   string
	Age    int
	School string
}

func declare() {
	var (
		exampleStruct MyExample
	)

	exampleStruct.Name = "sunkai"
	exampleStruct.Age = 100
	exampleStruct.School = "NJUE"

	fmt.Println(exampleStruct)
}

func main() {
	declare()
}

```

- 方法

  值传递一般获取值: func(m MyExample) GetAttrName()string {}

  指针传递一般修改值：func(m *MyExample)SetAttrName(newAge int){}

```go
package main

import "fmt"

type MyExample struct {
	Name   string
	Age    int
	School string
}

func declare() MyExample {
	var (
		exampleStruct MyExample
	)

	exampleStruct.Name = "sunkai"
	exampleStruct.Age = 100
	exampleStruct.School = "NJUE"

	return exampleStruct
}

func (m MyExample) GetAttrName() string {
	return m.Name
}

func (m *MyExample) SetAttrName(newAge int) {
	m.Age = newAge
}

func main() {
	myStruct := declare()
	fmt.Println(myStruct.GetAttrName())
	myStruct.SetAttrName(1000)
	fmt.Println(myStruct.Age)
}

```

- 匿名字段

隐式的引入一个结构体的所有字段

```go
type MyExample struct {
    Name string
    Age int
    School string
}
type OtherExample struct {
    MyExample
}

OtherExample 自动拥有 Name, Age, School 字段

```

2.函数

函数分为几类

- 匿名函数
- 结构体的方法
- 普通函数

写好函数注意几点

- 函数名称，要做到见文知意
- 参数
- 返回值
- 匿名函数：即没有函数名的函数，字面意思，一般用来处理简单功能

```go
func main() {
	var NoNameFunc = func(name string){
		fmt.Println(name)
	}

}
```

- 普通函数

```go
func GetMaxNumber(number, other float64) float64 {
	return math.Max(number, other)
}

- func 关键字
- GetMaxNumber 函数名称
- number, other 参数 类型 float64
- 返回值类型 float64
```

3.分支/判断/循环

- if判断
- if ... else /switch分支（当有多个if条件需要处理的时候，建议使用switch更好）
- for 循环

