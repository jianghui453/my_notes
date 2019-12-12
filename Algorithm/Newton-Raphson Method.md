# Newton-Raphson Method 牛顿迭代法

## Implementation

这里是一个简单的理解，实际原理挺复杂的。

- 对x的平方根的值一个猜想y。
- 通过执行一个简单的操作去得到一个更好的猜测：只需要求出y和x/y的平均值（它更接近实际的平方根值）。

## Code

```
import (
    "math"
)

func mySqr(n float64) float64 {
	ret := n/2
	for math.Abs((ret + n/ret)/2 - ret) >= 0.0000000001 {
		ret = math.Abs(ret + n/ret)/2
	}
	return ret
}
```

## Reference

1. [Wikipedia: Newton's method](https://en.wikipedia.org/wiki/Newton%27s_method)
