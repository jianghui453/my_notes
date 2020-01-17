# sync.Pool

Golang在 1.3 版本的时候，在sync包中加入一个新特性：Pool。 简单的说：就是一个临时对象池。

## 为什么需要sync.Pool？

保存和复用临时对象，减少内存分配，降低GC压力。对象越多GC越慢，因为Golang进行三色标记回收的时候，要标记的也越多，自然就慢了。

## 如何使用sync.Pool？

```
func main() {
	// 初始化一个pool
	pool := &sync.Pool{
		// 默认的返回值设置，不写这个参数，默认是nil
		New: func() interface{} {
			return 0
		},
	}

	// 看一下初始的值，这里是返回0，如果不设置New函数，默认返回nil
	init := pool.Get()
	fmt.Println(init)

	// 设置一个参数1
	pool.Put(1)

	// 获取查看结果
	num := pool.Get()
	fmt.Println(num)

	// 再次获取，会发现，已经是空的了，只能返回默认的值。
	num = pool.Get()
	fmt.Println(num)
}
```

## sync.Pool 在 fmt 包中的使用

```
type buffer []byte
// printer状态的结构体（）
type pp struct {
    ...
}

// pp的对象池， 《====这里用到了。
var ppFree = sync.Pool{
    New: func() interface{} { return new(pp) },
}

// 每次需要pp结构体的时候，都过sync.Pool进行获取。
func newPrinter() *pp {
    p := ppFree.Get().(*pp)
    p.panicking = false
    p.erroring = false
    p.fmt.init(&p.buf)
    return p
}
```

## 引用

1. [掘金：由浅入深聊聊Golang的sync.Pool](https://juejin.im/post/5d4087276fb9a06adb7fbe4a)