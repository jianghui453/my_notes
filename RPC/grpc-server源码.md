# grpc server 源码

1. 调用 net.Listen()，实现一个 net.Listener 。
2. 调用 grpc.NewServer() 实例化 Server ，初始化请求处理的 goroutine 以及对应的 channel 。
3. 调用 pb.RegisterGreeterServer()，在第二步生成 Server 实例中注册调用的函数。
4. 调用 Server.Serve() 函数接收数据，在收到数据后传入对应的 channel 中，由之前初始化的 gotoutine 处理。