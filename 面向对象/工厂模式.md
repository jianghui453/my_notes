# 工厂模式

实现一个函数，传入一个参数指定需要创建的对象类型，函数执行返回这个对象的实例。

go中一般使用 New*** 函数的形式实现

```
package simplefactory

import "fmt"

//hiAPI is one of API implement
type hiAPI struct{}

//Say hi to name
func (*hiAPI) Say(name string) string {
	return fmt.Sprintf("Hi, %s", name)
}

//HelloAPI is another API implement
type helloAPI struct{}

//Say hello to name
func (*helloAPI) Say(name string) string {
	return fmt.Sprintf("Hello, %s", name)
}

//API is interface
type API interface {
	Say(name string) string
}

//NewAPI return Api instance by type
func NewAPI(t int) API {
	if t == 1 {
		return &hiAPI{}
	} else if t == 2 {
		return &helloAPI{}
	}
	return nil
}
```

## 引用

1. [简单工厂模式](https://github.com/senghoo/golang-design-pattern/tree/master/00_simple_factory)
