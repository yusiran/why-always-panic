# Why Always Panic ???

- panic records for golang

## panic: assignment to entry in nil map

```golang
var param map[string]string
param["a"] = "b"
```

报错了！未初始化的map值为nil，不可增加元素

正确用法：
```golang
var param map[string]string
param = make(map[string]string])
param["a"] = "b"
```

## echo接收json格式的post参数返回失败

```golang
type User struct {
    id int `json:"id" form:"id"`
    name string `json:"name" form:"name"`
}

func GetUser(c echo.Context) error {
    u := new(User)
    c.Bind(u)
    return c.JSON(200, u)
}
```

postman中返回结果是个空数组。User struct中的id和name小写字母开头，是私有变量，返回的时候不能用

正确用法：
```golang
type User struct {
    Id int `json:"id" form:"id"`
    Name string `json:"name" form:"name"`
}
```

## func的[]interface{}类型参数自动转换问题

```golang
func Contain(data []interface{}, target interface{}) bool {
    result := false
    for _, item := range data {
        if item == target {
            result = true
            break
        }
    }
    return result
}

func main() {
    strings := []string{"a", "b", "c"}
    target := "d"
    c := Contain(strings, target)
    fmt.Println(c)
}
```

会报语法错误：```cannot use names (type []string) as type []interface{} in argument to Contain```

原因：go不会对interface{}类型的slice做类型转换

官方解释：https://github.com/golang/go/wiki/InterfaceSlice

正确用法：
1.遍历strings，做类型转换，把元素导入```[]interface```类型的slice中，再调用Contain()
2.Contain的data参数改成```[]string```类型


## evaluated but not used

```golang
append(strSlice, "a")
```

报错append() evaluated but not used

原因：append()会返回append之后的strSlice，需要使用

正确用法：
```golang
strSlice = append(strSlice, "a")
```

## append()之前初始化的注意事项

```golang
myArr := make([]string, 1)
myArr = append(myArr, "item")
fmt.Println(len(myArr))
```

打印出来是2

解释：make时已为myArr初始化了1个元素，append不会覆盖已有的元素

正确用法：
```golang
myArr := make([]string, 0)
myArr = append(myArr, "item")
```
或者
```golang
var myArr []string
myArr = append(myArr, "item")
```

## github.com/spf13/cast包的cast.ToInt()方法的坑

```golang
fmt.Println(cast.ToInt("009"))
```

打印出来是0

解释：cast.ToInt("009")在参数为string类型时，底层用的是strconv.ParseInt("009", 0, 0)，其中第二个参数表示的是进制，进制为0时会根据第一个参数的值来判断进制。判断规则为：以0开头，0b开头二进制，0o开头八进制，0x开头十六进制，其他默认八进制；非0开头默认十进制。所以"009"会使用八进制进行转换

正确用法：
```golang 
strconv.Atoi("009")
```
或者
```golang 
strconv.ParseInt("009", 10, 0)
```
