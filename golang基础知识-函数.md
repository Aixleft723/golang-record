##### golang基础知识

函数

- 函数的定义
- 参数：定参，不定参数
- 返回值：单个返回值，多个返回值， 命名返回值
- 值传递和指针传递
- 函数作为参数
- 匿名函数
- main 和 init 函数

1.函数的定义

- 关键字 func

```go
//declare

func example() {
	fmt.Println("hello world")
}

//func main() {
	example()
}
```

2.函数参数：单个参数，不定参数

```go
//arg 
func exampleOneArg(arg int) {
	fmt.Println(arg + 1)
}

//arg list
func exampleListArg(args ...int) {
	for index, value := range args {
		fmt.Println(index, value)
	}
}
```

3.返回值：单个返回值，多个返回值，命名返回值

```go
//one response

func exampleOneResponse(arg int) int {
	return arg + 100
}

//multiple responses

func exampleTwoResponses(arg int)(int, int) {
	return arg + 10, arg + 20
}

//name response

func exampleNameResponse(arg int)(sum int) {
	sum = arg + 100
	return
}

func main() {
	exampleOneResponse(100)
	exampleTwoResponses(10)
	exampleNameResponse(10)
}
```

4.值传递和指针传递

值传递和指针传递针对的是函数的传入参数究竟是拷贝入参的值进行操作还是拷贝入参的内存地址，带来的效果就是一个能改变传入的参数的值，一个不能改变传入参数的值

```go
// copy value

func exampleValueCopy(arg int) int {
    arg = arg + 1
    return arg
}

// copy address

func exampleValueAddress(arg *int) int {
    *arg = *arg + 1
    return *arg

}

// main
func main() {

    var arg int = 10
    exampleValueCopy(arg)
    fmt.Println(arg) // 10
    exampleValueAddress(&arg)
    fmt.Println(arg) // 11
}

```

5.函数作为参数

```go
// func as arg

func funcArg(arg int) int {
    return arg * 100
}

func exampleFuncAsArg(arg int, function func(int) int) int {
    return arg + function(arg)

}

// main
func main() {

    fmt.Println(exampleFuncAsArg(10, funcArg)) // 1010
}

```

6.匿名函数

```go
// un name func

var NoNameFunc = func(arg int) int { return arg * 1000 }


// main
func main() {

    fmt.Println(NoNameFunc(10)) // 10000
}


```

7.main和init函数

- 这两个函数没有入参和返回值
- 一个工程有且只有一个 main 函数作为程序的主入口
- 一个工程可以有多个init 函数，执行顺序和包的导入顺序相关



