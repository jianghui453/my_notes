# grpc 概念

## stub

pb 协议编译后会生成服务对应的 service 和 client 类型。分别称为 service stub 和 client stub。

## channelz

主要用于信息收集，可以用于 debug ，以及判断程序异常。

## resolver

地址解析器，通过地址解析器可以实现注册服务。

### watcher

1. 客户端调用 Dail()；
2. 调用 DialContext()；
3. 调用 newCCResolverWrapper()；
4. 调用 resolver.Builder.Build()；
5. 调用 resolver.Builder.watcher()；

在调用 resolver.Build 的时候会启动 resolver.watcher() 监听服务变化。

## balancer

负载均衡器，实现服务集群的负载均衡调用。
