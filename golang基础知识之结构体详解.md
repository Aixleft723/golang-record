#### golang基础知识之结构体

###### 0.引入

golang中用来表示单一的数据类型可以使用变量声明的方式

```go
var number int
var name string
var length float64

```

golang中用来表示同一数据类型的集合

```go
var numbers [3]int
var sum []int
var peoples map[string]string

```

如果需要表示不同数据类型的集合应该如何表示呢？

- 结构体

**结构体可以用来表示不同的数据类型的集合, 同时可以表示用户自定义类型**

1.定义和声明

- 按顺序初始化
- 按任意顺序初始化
- 字段赋值
- new函数分配指针

2.标签

- 字段访问
- 方法：值传递和指针传递
- 函数和方法的区别
- 组合：内嵌结构体，内嵌匿名成员
- 格式化显示结构体

###### 1.定义和声明

使用type 和struct 关键字，golang中，结构体也是一种数据类型，因此可以像下面这样声明

```go
type name struct{}
var infoName name //声明一个结构体类型的变量

```

结构体在golang中又可以当作类，因此拥有方法

```go
type Name struch {}

func (N Name) ResponseNumber() int {
	
}
```

结构体方法调用是 “.”

```
func main() {
	var name Name
	res := name.ResponseNumber()
}
```

声明示例：

```go
type Information struct {
    Name    string
    Age     int
    School  string
    Married bool
    Habits  []string
    Infor   map[string]string
}

func exampleInformationDeclare() {
    // one
    var (
        exampleInformationOne Information
        Infor                 map[string]string
    )
    Infor = make(map[string]string)
    Infor["key"] = "hello-world"
    exampleInformationOne = Information{
        "xiewei", 18, "shanghaiUniversity", true,
        []string{"1", "2", "3"}, Infor,
    }
    fmt.Println(exampleInformationOne, exampleInformationOne.Name)

    // two
    var exampleInformationTwo Information
    exampleInformationTwo.Name = "golang"
    exampleInformationTwo.School = "Google"
    exampleInformationTwo.Age = 10
    exampleInformationTwo.Infor = Infor

    fmt.Println(exampleInformationTwo, exampleInformationTwo.Married)

    // three
    var exampleInformationThree Information
    exampleInformationThree = Information{
        Name:    "Python",
        School:  "Gudio school",
        Age:     18,
        Habits:  []string{"1", "2"},
        Infor:   Infor,
        Married: true,
    }

    fmt.Println(exampleInformationThree, exampleInformationThree.School)

    // four

    var exampleInformationFour *Information
    exampleInformationFour = new(Information)
    exampleInformationFour.Name = "Java"

    fmt.Println(exampleInformationFour, exampleInformationFour.School)
}



func main() {
    exampleInformationDeclare()
}
```

###### 2.标签

这种带标签的结构体的定义，在转换为json格式的数据的时候会自动将对应的字段转换为标签中的字段：

比如：Name 转换为 name 字段。

标签字段不能在编程中实际使用。

比如：可以这样: `InformationWithLabel.Name`

不可以这样：`InformationWithLabel.name`

```go
type InformationWithLabel struct {
	Name string `json:"name"`
	Age int `json:"age"`
}
```

###### 3.字段访问

结构体内的是一系列相关字段的集合。如果定义了一个结构体如何访问?  可以使用点号获取结构体的字段。

```go
type InformationWithLabel struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

func exampleInformationWithLabelFiled() {
    var example InformationWithLabel
    example = InformationWithLabel{
        Name: "xiewei",
        Age:  18,
    }
    fmt.Println(example.Name, example.Age)
}

```

###### 4.方法

方法根据传入的参数的不同，又分为：值传递 和 指针传递。两者的效果就是：值传递不可改变值，指针传递可以改变值

```go
type Response struct {
    Code    int
    Result  []byte
    Headers map[string]string
}

func (r Response) GetAttrCode() int {
    return r.Code
}
func (r Response) GetAttrResult() []byte {
    return r.Result
}

func (r Response) GetAttrHeader() map[string]string {
    return r.Headers
}

func (r *Response) SetCode(code int) {
    r.Code = code
}

func (r *Response) SetHeaders(key, value string) {
    r.Headers[key] = value
}

func exampleResponse() {
    var (
        response Response
        headers  map[string]string
    )
    headers = make(map[string]string)
    headers["Server"] = "GitHub.com"
    headers["Status"] = "Ok"
    response.Headers = headers
    response.Code = 200
    response.Result = []byte("hello world")

    fmt.Println(response.GetAttrCode())
    fmt.Println(response.GetAttrHeader())
    fmt.Println(response.GetAttrResult())

    response.SetCode(404)
    fmt.Println(response)

    response.SetHeaders("Status", "failed")
    fmt.Println(response)
}


func main() {
    exampleResponse()
}


>>>

200
map[Server:GitHub.com Status:Ok]
[104 101 108 108 111 32 119 111 114 108 100]
{404 [104 101 108 108 111 32 119 111 114 108 100] map[Server:GitHub.com Status:Ok]}
{404 [104 101 108 108 111 32 119 111 114 108 100] map[Status:failed Server:GitHub.com]}
```

区别：

- 结构体的方法如何定义
- 值传递的适用于取值
- 指针传递适用于更改字段的值

###### 5.函数和方法的区别

代码：

```go
func NormalFunc(arg int) int {
    return arg + 1
}


func (r *Response) SetCode(code int) {
    r.Code = code
}
```

Go 方法是作用在接收者（receiver）上的一个函数。接收者可以是几乎任何类型。但一般选择 结构体 作为接收者。

###### 6.组合

**匿名字段**

在 Golang 中可以通过结构体的组合实现类的继承。

即：将一个结构体A当成另一个结构体B的匿名字段，则 这个结构体B自动拥有A的所有字段和方法

```go
type Response struct {
    Code    int
    Result  []byte
    Headers map[string]string
}

func (r Response) GetAttrCode() int {
    return r.Code
}
func (r Response) GetAttrResult() []byte {
    return r.Result
}

func (r Response) GetAttrHeader() map[string]string {
    return r.Headers
}

func (r *Response) SetCode(code int) {
    r.Code = code
}

func (r *Response) SetHeaders(key, value string) {
    r.Headers[key] = value
}

type Requests struct {
    Url    string
    Params string
}

type CollectionRequests struct {
    CollectionNumber int
    Requests
    Response
}

func exampleCollectionRequests() {

    var collectionRequests CollectionRequests
    collectionRequests.CollectionNumber = 10
    collectionRequests.Url = "https://www.example.com"
    collectionRequests.Params = "name"
    collectionRequests.Code = 201
    collectionRequests.Result = []byte("hello Golang")

    var headers map[string]string
    headers = make(map[string]string)
    headers["status"] = "Good"
    collectionRequests.Headers = headers
    fmt.Println(collectionRequests)

    fmt.Println(collectionRequests.GetAttrCode())
}

func main() {
    exampleCollectionRequests()
}

>>>

{10 {https://www.example.com name} {201 [104 101 108 108 111 32 71 111 108 97 110 103] map[status:Good]}}
201
```

上文`CollectionRequests` 拥有两个匿名字段`Requests、Response` ，则自动拥有这个两个结构体的字段和方法

**内嵌结构体**

这个组合的形式会遇到两个问题：

- 字段相同怎么办？即结构体A 有字段 a, 结构体 B 也有字段 a。怎么处理？
- 方法相同怎么办？即结构体A 有方法 methodOne, 结构体 B 也有方法 methodOne。怎么处理？

应该尽量避免命名冲突。同时可以使用多个点号的方法访问字段。方法则优先使用结构体B 的。

```
type OtherRequests struct {
    Request Requests
    Resp    Response
    Code    int
}
func (o OtherRequests) GetAttrCode() {
    fmt.Println(fmt.Sprintf("Outer Code = %d", o.Code))
    fmt.Println(fmt.Sprintf("inner Code = %d", o.Resp.Code))
}
func exampleOtherRequests() {
    var other OtherRequests
    other.Code = 201
    other.Resp.Code = 202
    fmt.Println(other)
    other.GetAttrCode()
    fmt.Println(other.Resp.GetAttrCode())
}
func main() {
    exampleOtherRequests()
}


>>>
{{ } {202 [] map[]} 201}
Outer Code = 201
inner Code = 202
202
```

通过组合结构体的方式，可以实现类中的继承和多态

###### 7.格式化结构体

**Golang 中只需要实现一个名为 String 的方法即可格式化显示结构体。**

```go
type OtherRequests struct {
    Request Requests
    Resp    Response
    Code    int
}

func (o OtherRequests) String() string {
    return fmt.Sprintf("Request = %v , Response = %v , Code = %d", o.Request, o.Resp, o.Code)
}

func exampleOtherRequests() {
    var other OtherRequests
    other.Code = 201
    other.Resp.Code = 202
    fmt.Println(other)
    other.GetAttrCode()
    fmt.Println(other.Resp.GetAttrCode())
}

func main() {

    exampleOtherRequests()
}

>>>
Request = { } , Response = {202 [] map[]} , Code = 201
Outer Code = 201
inner Code = 202
202
```

当结构体实现了String 方法之后，使用fmt.Println 打印就能看出效果