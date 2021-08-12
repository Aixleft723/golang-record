### golang基础知识

###### 主要包括：

- 常量
- 变量
- 字符串
- 数组
- 切片
- MAP
- 结构体
- 函数
- 方法
- 分支
- 循环

###### 一、变量：

注意事项：

- 命名

  原则：

1. 见文知意
2. 规范：golang中采用驼峰命名标准：numberClass
3. 首字母大写：外部可访问，首字母小写：仅限于内部访问
4. 长度要适中
5. 避免使用关键字

```golang
var ok bool
var value string
var number int
```

- 声名及初始化

​       原则：

1. 明确数据类型
2. 少使用隐式声名

```golang
# Better
var Name string

#Not Better 
Name := "sunkai"
```

在函数内部才可以使用简短变量声明

- 使用

​        原则：

1.变量声明靠近使用处

2.多个变量声明可以放在一起

```golang
var (
	url string
	result []int
	err error
)

url := "http://www.baidu.com"
if result, err := http.Get(url); err != nil {
	xxx
}
```

3.常用的bool 值变量名称：

- done
- found
- ok
- success
- flag

4.使用对仗：

- begin/start
- first/last
- locked/unlocked
- min/max
- next/previous
- old/new
- opened/closed
- visible/invisible
- source/target
- source/destination
- up/down

###### 二、常量

关键字：const

```golang
const name = "sunkai"
```

###### 三、字符串

- 短字符串

```golang
var Help string

Help = "Thank you for Your help."
```

- 长字符串

```golang
var HelpMessage string

HelpMessage = `
Hello Everone:
  Welcome to here.
  Today, we will learn how to learn golang.
`
```

字符串常用操作：

库：strings

内置库的使用注意两点：

- 函数的输入值及类型
- 函数的输出值及类型
- Contains 判断是否包含字符串

```golang
func Contains(s, substr string) bool {

}
```

- Count: 输出字符串中字符个数

```golang
func Count(s, sep string) int {

}
```

- Index: 返回字符串第一次出现的位置，不存在则返回-1

```golang
func Index(s, sep string) int {

}
```

- Join: 按给定的字符串切片以及指定的分隔符拼接成一个字符串

```go
func Join(a []string, sep string) string {

}
```

- LastIndex: 字串最后一次出现的位置

```golang
func LastIndex(s, sep string) int {

}
```

- `ToLower`: 將字符串全部小寫

```go
func ToLower(s string) string
```

- `ToUpper`: 將字符串全部大寫

```go
func ToUpper(s string) string
```

- `Repeat`: 重復字符串

```go
func Repeat(s string, count int ) string
```

- `Replace`: 替換

```go
func Replace(s, old, new string, n int) string
```

- `TrimSpace`: 去除字符串前後空格

```go
func TrimSpace(s string) string
```

- `Split`: 切割

```go
func Split(s, sep string) []string 
```

