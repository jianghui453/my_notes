最基本的处理器与内存以及io设备的架构：

![](images/cpu_and_memory/1.png)

- 近期的RAM需要两条独立总线(或者说通道，DDR2就是这么叫的)，可以实现带宽加倍。
- 内存寻址
- 在操作系统中，内存以页单位粒度管理。一个进程的物理内存分配可能不是连续的内存空间。就需要通过页的编号对应地址指针进行寻址。