# 单例模式

 一个类只有一个实例，通过静态类型或者全局变量实现。如：http.DefaultClient。

```
package singleton

import "sync"

//Singleton 是单例模式类
type Singleton struct{}

var singleton *Singleton
var once sync.Once

//GetInstance 用于获取单例模式对象
func GetInstance() *Singleton {
	once.Do(func() {
		singleton = &Singleton{}
	})

	return singleton
}
```
