# TCP keepAlive

在TCP中有一个Keep-alive的机制可以检测死连接，原理很简单，TCP会在空闲了一定时间后发送数据给对方：

1.如果主机可达，对方就会响应ACK应答，就认为是存活的。
2.如果可达，但应用程序退出，对方就发RST应答，发送TCP撤消连接。
3.如果可达，但应用程序崩溃，对方就发FIN消息。
4.如果对方主机不响应ack, rst，继续发送直到超时，就撤消连接。这个时间就是默认的二个小时。

## 如何配置

KeepAlive默认不是开启的，如果想使用KeepAlive，需要在你的应用中设置SO_KEEPALIVE才可以生效。
```
setsockopt(s, SOL_SOCKET, SO_KEEPALIVE, &optval, sizeof(optval));
```

### 在Linux中我们可以通过修改 /etc/sysctl.conf 的全局配置：

net.ipv4.tcp_keepalive_time=7200
net.ipv4.tcp_keepalive_intvl=75
net.ipv4.tcp_keepalive_probes=9

## 引用

1. [聊聊 TCP 中的 KeepAlive 机制](https://zhuanlan.zhihu.com/p/28894266)
