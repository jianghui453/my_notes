# Interface

#### 可取代性（substitutability）

可以把一种类型替换为满足同一接口的另一种类型的特性。

#### 接口值

一个接口类型的值有两个部分：一个具体类型和该类型的一个值。称为接口的**动态类型**和**动态值**。

接口的零值就是把它的动态类型的值都设置为nil。
![](images/interface/1.png)
一个接口值是否是nil取决于它的动态类型，所以现在是一个nil的接口值。