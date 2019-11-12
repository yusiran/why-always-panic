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
