# chan

## 注意点

1. 向一个已关闭的 channel 写入数据会报错，但从一个已关闭的 channel 读取数据不会报错；
2. 主程序在读取一个没有生产者的 channel 时会被判断为死锁，如果是在新开的协程中是没有问题的，同理主程序在往没有消费者的协程中写入数据时也会发生死锁；
3. 当通道被两个协程操作时，如果一方因为阻塞导致另一放阻塞则会发生死锁，如下代码创建两个通道，开启两个协程 (主协程和子协程)，主协程从 c2 读取数据，子协程往 c1，c2 写入数据，因为 c1，c2 都是无缓冲通道，所以往 c1 写时会阻塞，从 c2 读取时也会会阻塞，从而发生死锁；

## 在 select 中监听 channel

1. 如果有默认值，没有一个 channel 中有数据时会立即返回默认值，否则阻塞；
2. 如果监听多个 channel，且多个 channel 有数据，则随机返回其中一个数据；

## 代码

```
type hchan struct {
	qcount   uint           // total data in the queue
	dataqsiz uint           // size of the circular queue
	buf      unsafe.Pointer // points to an array of dataqsiz elements
	elemsize uint16
	closed   uint32
	elemtype *_type // element type
	sendx    uint   // send index
	recvx    uint   // receive index
	recvq    waitq  // list of recv waiters
	sendq    waitq  // list of send waiters

	// lock protects all fields in hchan, as well as several
	// fields in sudogs blocked on this channel.
	//
	// Do not change another G's status while holding this lock
	// (in particular, do not ready a G), as this can deadlock
	// with stack shrinking.
	lock mutex
}
```
