# linux 实时调度策略

SCHED_FIFIO与SCHED_RR两种实时调度策略：

- FIFO：只能被高优先级的实时进程抢占
- RR：耗尽事先分配的时间后就不能再运行了。轮流调度其他同一优先级的实时进程。

## 引用

- [linux实时调度策略](https://blog.csdn.net/xiang_shao344/article/details/82748721)