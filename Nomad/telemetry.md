# telemetry（遥测数据）

Nomad client和server收集与系统性能相关的各种runtime指标。可以基于这些指标调整Nomad集群。

Server leader和follower有相同的配置以及针对各自角色的配置。Client针对host和allocation/task有单独的指标，必须明确启动这两个指标。还有所有服务器和客户端通用的runtime指标。

默认情况下，Nomad agent每隔1s收集一次telemetry。Nomad支持仪表、计数器和计时器。

## 从Nomad获取metric
- 查询/metric API。
- 发送USR1信号到Nomad进程。这会将数据保存到STDERR上。
- 配置Nomad自动将数据发送到第三方。
