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
