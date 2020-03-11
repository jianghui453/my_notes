# cgo

## 原理

其实cgo就是先由编译器识别出import "C"的位置，然后在其上的注释中提取C代码，最后调用C编译器进行分开编译

## 写法

### 静态库的链接

静态库的连接还是很简单的。我们要理解cgo的原理。其实cgo就是先由编译器识别出import "C"的位置，然后在其上的注释中提取C代码，最后调用C编译器进行分开编译。所以在cgo中可以指定编译参数。
连接静态库时必须添加参数。具体方法如下：
```
Golang
// #cgo LDFLAGS: -L. -lhello
// #include "hello.h"
```
这条宏就为代码加入了libhello静态库的连接。在编译时，会先从-L后面的目录查找libhello.a，如果没有找到就到系统默认的目录下查找。
我们只需写好hello.c和hello.h,然后执行

### 动态库的链接

动态库没法在编译时进行链接，只能在代码中调用。在*nix下我们通常使用dlfcn.h中的一系列函数，举个例子：
```
Golang
// #cgo LDFLAGS: -ldl
// #include <dlfcn.h>
// #include <stdlib.h>
import "C"
import "unsafe"

libname := C.CString("example.so")
defer C.free(unsafe.Pointer(libname))
lib := C.dlopen(libname, C.RTLD_NOW)
defer C.dlclose(lib)
```
这样就算是打开了动态库，之后的调用请参考man手册，其实用起来是很麻烦的。在获得了函数指针之后，使用syscall中的一系列函数进行调用：
```
Golang
syscall.Syscall
syscall.Syscall6
syscall.Syscall9
syscall.Syscall12
syscall.Syscall15
```

## 引用

1. [cgo](https://www.jianshu.com/p/45e4aa2e7c2e)
1. [使用Cgo的一点总结](http://blog.ihuxu.com/the-note-of-cgo-usage/)