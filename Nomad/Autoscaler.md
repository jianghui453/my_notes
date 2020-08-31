# Autoscaler

Nomad的集群自动水平扩展器。

## 应用水平扩展

应用的水平扩展是通过自动控制应用程序的实例数量以使其具有足够的工作吞吐量以满足服务级别协议（SLA）。可以通过相关指标（cpu和内存的利用率、连接数等）修改任务组中的分配数量来实现应用的水平扩展。

## 集群水平扩展

集群水平扩展是在集群中添加或删除Nomad客户端以确保计划的应用程序有适当数量的集群资源过程。这是通过与远程提供程序进行交互以基于度量（例如剩余的可用可调度CPU或内存）启动或终止新的Nomad客户端来实现的。通过 使用针对Nomad群集的策略配置自动扩展程序代理，可以启用群集扩展。