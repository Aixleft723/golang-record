

##### golang基础知识：

- 数组
- 切片
- MAP

###### 数组：

- 固定长度列表在golang里称为数组，一段连续内存
- 不固定长度称为切片

``` go
var numberList = [3]string

numberList = [3]string{"python", "golang", "c"}	
	
```

- 获取数组长度： len
- 获取数组容量： cap

编程中,凡是可以使用`if...else`的都可以使用数组来遍历,再对遍历的值进行操作

###### 切片：

- len获取长度
- cap获取容量
- 常见操作
- slice

```golang
func main(){
    var (
        value []string
    )

    value = []string{
        "sunkai",
        "pero",
        "faker",
    }

    fmt.Print(value[1:])
}

>>> [pero faker]

```

- append

```golang
func main(){
    var (
        value []string
    )

    value = []string{
        "xieWei",
        "weiWei",
        "wuWei",
    }

    value = append(value, "shenWei")
    fmt.Println(value)
}

>>> [xieWei weiWei wuWei shenWei]

```

- range操作

```golang
func main(){
    var (
        value []string
    )

    value = []string{
        "xieWei",
        "weiWei",
        "wuWei",
    }
    for index, i:= range value{
        fmt.Println(index, i)
    }
}

>>> 
0 xieWei
1 weiWei
2 wuWei

```

作者：谢伟
链接：https://zhuanlan.zhihu.com/p/37219485
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



###### 6. Map

字典: 键值型

- 声明

```golang
func mapDeclare(key string, value string)map[string]interface{}{
    var (
        body map[string]interface{}
    )
    body = make(map[string]interface{})
    body[key] = value
    return body
}

func main(){
   fmt.Println(mapDeclare("name", "xieWei"))
}

>>> map[name:xieWei]
```

- 遍历

```golang
func main(){
    var (
        body map[string]interface{}
    )
    body = make(map[string]interface{})
    body["name"] = "xiewei"
    body["age"] = 26
    body["university"] = "shanghaiUniversity"
    rangeMap(body)
}
func rangeMap(body map[string]interface{}) {
    for key, value := range body{
        fmt.Println(key, value)
    }
}

>>> 
name xiewei
age 26
university shanghaiUniversity
```

