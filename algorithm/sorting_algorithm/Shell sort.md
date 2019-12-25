# Shell sort

Shellsort, also known as Shell sort or Shell's method, is an in-place comparison sort. It can be seen as either a generalization of sorting by exchange (bubble sort) or sorting by insertion (insertion sort). The method starts by sorting pairs of elements far apart from each other, then progressively reducing the gap between elements to be compared. Starting with far apart elements, it can move some out-of-place elements into position faster than a simple nearest neighbor exchange. Donald Shell published the first version of this sort in 1959. The running time of Shellsort is heavily dependent on the gap sequence it uses. For many practical variants, determining their time complexity remains an open problem.

## Implementation

![](../../Images/Algorithm/SortingAlgorithm/Shell%20sort.gif)

```
func shellSort(nums []int) {
	l := len(nums)
	for i := l/2; i > 0; i /= 2 {
		for j := i; j < l; j++ {
			k, v := j, nums[j]
			for ; k >= i && nums[k-i] > v; k -= i {
				nums[k] = nums[k-i]
			}
			nums[k] = v
		}
	}
}
```

## Reference

1. [Wikipedia: Shellsort](https://en.wikipedia.org/wiki/Shellsort)
1. [cnblogs:十大经典排序算法（动图演示）](https://www.cnblogs.com/onepixel/p/7674659.html)
1. [GeeksforGeeks: ShellSort](https://www.geeksforgeeks.org/shellsort/)