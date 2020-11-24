# Pipeline concepts

## Pipeline

Pipeline是用户定义的CD管道模型。Pipeline的代码定义了创建、测试、部署应用过程。

## Node

Node是Jenkins环境的一部分，也是执行Pipeline的容器。

## Stage

Stage是整个Pipeling中的不同的子集，可以使用插件将Pipeline过程可视化。

## Step

Step指定Jenkins在什么什么情况下进行哪种操作。例如在执行make命令时使用sh step：sh 'make'。