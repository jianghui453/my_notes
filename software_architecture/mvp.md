MVP 模式将 Controller 改名为 Presenter，同时改变了通信方向。

![imt](images/mvp/1.png)

1. 各部分之间的通信，都是双向的。
2. View 与 Model 不发生联系，都通过 Presenter 传递。
3. View 非常薄，不部署任何业务逻辑，称为”被动视图“（Passive View），即没有任何主动性，而 Presenter 非常厚，所有逻辑都部署在那里。

### 引用

1. [阮一峰的网络日志：MVC，MVP 和 MVVM 的图示](https://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)
