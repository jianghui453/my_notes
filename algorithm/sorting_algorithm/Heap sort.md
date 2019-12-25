# Heap sort

In computer science, heapsort is a **comparison-based** sorting algorithm. Heapsort can be thought of as an improved selection sort: like that algorithm, it divides its input into a sorted and an unsorted region, and it iteratively shrinks the unsorted region by extracting the largest element and moving that to the sorted region. The improvement consists of the use of a heap data structure rather than a linear-time search to find the maximum.

## Implementation

![](../../Images/Algorithm/SortingAlgorithm/Heap%20sort.gif)

```
func heapSort(nums []int) {
	nLen := len(nums)
	if nLen < 2 {
		return
	}
	for i := nLen/2 - 1; i >= 0; i-- {
		heapify(nums, nLen, i)
	}

	nums[nLen-1], nums[0] = nums[0], nums[nLen-1]
	for i := nLen - 2; i >= 0; i-- {
		heapify(nums[:i+1], i+1, 0)
		nums[i], nums[0] = nums[0], nums[i]
	}
}


func heapify(nums []int, n, i int) {
	left := 2*i + 1
	right := 2*i + 2

	if right < n && nums[right] > nums[i] {
		nums[right], nums[i] = nums[i], nums[right]
		heapify(nums, n, right)
	}

	if left < n && nums[left] > nums[i] {
		nums[left], nums[i] = nums[i], nums[left]
		heapify(nums, n, left)
	}
}
```

## Reference

1. [Wikipedia: Heap sort](https://en.wikipedia.org/wiki/Heap_sort)
1. [cnblogs:十大经典排序算法（动图演示）](https://www.cnblogs.com/onepixel/p/7674659.html)