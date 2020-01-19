# epoll

epoll is a Linux kernel system call for a scalable I/O event notification mechanism, first introduced in version 2.5.44 of the Linux kernel mainline.[] Its function is to monitor multiple file descriptors to see whether I/O is possible on any of them. It is meant to replace the older POSIX select(2) and poll(2) system calls, to achieve better performance in more demanding applications, where the number of watched file descriptors is large (unlike the older system calls, which operate in O(n) time, epoll operates in O(1) time).

epoll is similar to FreeBSD's kqueue, in that it consists of a set of user-space functions, each taking a file descriptor argument denoting the configurable kernel object, against which they cooperatively operate. epoll used **red-black tree (RB-tree)** data structure to keep track of all file descriptors that are currently being monitored.

## 引用

- [Wikipedia: epoll](https://en.wikipedia.org/wiki/Epoll)