##### golang错误处理

- err变量
- error接口
- panic
- recover和defer

###### 0.err变量

在golang 中如果需要进行错误处理，一般都默认函数的最后一个返回值是 err

```go
func calres(n int) (int, error) {
	return n + 1, nil
}
```

比如看内置读取文件的`ioutil.ReadAll` 的定义

```go
func ReadAll(r io.Reader) ([]byte, error) {
    return readAll(r, bytes.MinRead)
}
```

可以看出，一般函数的最后一个返回值定义为 err, 可以通过这个 错误信息判断函数是否执行正确。

```go
if err != nil {
	//执行相应的动作
}

```

###### 1.error接口

```go
type error interface {
	Error() string // Error方法，返回值是string
}
```

内置 errors 包实现了这个接口：

```go
package errors

// New returns an error that formats as the given text.
func New(text string) error {
    return &errorString{text}
}

// errorString is a trivial implementation of error.
type errorString struct {
    s string
}

// 实现了 error 接口的Error 方法
func (e *errorString) Error() string {
    return e.s
}

// 带格式输出的 error 
func Errorf(format string, a ...interface{}) error {
    return errors.New(Sprintf(format, a...))
}

```

所以可以通过下面两种方法创建包含错误文本的 error 对象：

```go
errors.New
fmt.Errorf 带格式化输出的 error 对象

```

```go
// errors.New
func exampleReadAllError() error {
    _, err := ioutil.ReadAll(nil)
    return err
}

// fmt.Errorf

func exampleErrorf(arg int) error {
    return fmt.Errorf("%d", arg)

}

```

###### 2.panic

在 golang 中遇到错误，需要强制退出程序，可以使用 panic 。panic 接收的参数是任意类型

```go
func panic(v interface{})
```

```go
func examplePanic() {
    panic(nil)
}

func main(){
      examplePanic()
}

>>>
panic: nil

goroutine 1 [running]:
main.examplePanic()
    C:/Users/wuxiaoshen/go/src/go-example-for-live/five_learning/main.go:28 +0x3a
main.main()
    C:/Users/wuxiaoshen/go/src/go-example-for-live/five_learning/main.go:37 +0x27

```

上文中调用 examplePanic 函数会抛出异常，直接终止程序的执行。panic 传入的参数是错误信息。比如上文 `nil`,除非你确定需要终止程序运行，否则不太使用 panic

###### 3.recover

recover 可以接收返回的异常信息，通常和 defer 一起使用。

```go
func examplePanic() {
    panic("123")
}

func main() {
    defer func() {
        if err := recover(); err != nil {
            fmt.Println(fmt.Sprintf("recieve error: %s", err))
        }
    }()
    examplePanic()
}

>>>
recieve error: 123

```

上文操作后，examplePanic 抛出的异常被 recover 接收到，再进行了后面的处理。上文执行就没有错误提示，当多个 panic 抛出异常时， recover 接收第一个 panic 抛出的异常。

```
func examplePanic() {
    panic("123")
}

func examplePanicOther() {
    panic("1234")
}

func main() {
    defer func() {
        if err := recover(); err != nil {
            fmt.Println(fmt.Sprintf("recieve error: %v", err))
        }
    }()
    examplePanic()
    examplePanicOther()
    examplePanic()

}

>>>
recieve error: 123

```

