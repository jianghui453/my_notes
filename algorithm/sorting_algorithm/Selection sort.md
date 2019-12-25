# Selection sort

In computer science, selection sort is a sorting algorithm, specifically an **in-place** comparison sort. It has **O(n2)** time complexity, making it inefficient on large lists, and generally performs worse than the similar insertion sort. Selection sort is noted for its simplicity, and it has performance advantages over more complicated algorithms in certain situations, **particularly where auxiliary memory is limited.**

## Implementation

![](../../Images/Algorithm/SortingAlgorithm/Selection%20sort.gif)

```
func selectSort(nums []int) {
	nLen := len(nums)
	for i := 0; i < nLen-1; i++ {
		minIdx := i
		minVal := nums[i]
		for j := i + 1; j < nLen; j++ {
			if nums[j] < minVal {
				minVal = nums[j]
				minIdx = j
			}
		}
		nums[i], nums[minIdx] = nums[minIdx], nums[i]
	}
}
```

## Reference

1. [Wikipedia: Selection sort](https://en.wikipedia.org/wiki/Selection_sort)
1. [cnblogs:十大经典排序算法（动图演示）](https://www.cnblogs.com/onepixel/p/7674659.html)