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
