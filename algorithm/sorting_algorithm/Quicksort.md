# Quicksort

Quicksort (sometimes called partition-exchange sort) is an efficient sorting algorithm, serving as a systematic method for placing the elements of a random access file or an array in order. Developed by British computer scientist Tony Hoare in 1959[1] and published in 1961, it is still a commonly used algorithm for sorting. When implemented well, it can be about two or three times faster than its main competitors, merge sort and heapsort.

## Implementation

![](../../Images/Algorithm/SortingAlgorithm/Quicksort.gif)

```
func quickSort(nums []int) {
	l := len(nums)
	if l <= 1 {
		return
	}

	newnums := make([]int, l)
	left, right := 0, l-1
	for i := 1; i < l; i++ {
		if nums[i] > nums[0] {
			newnums[right], right = nums[i], right-1
		} else if nums[i] < nums[0] {
			newnums[left], left = nums[i], left+1
		}
	}
	for i := left; i <= right; i++ {
		newnums[i] = nums[0]
	}
	
	copy(nums, newnums)
	quickSort(nums[: left])
	if right < l-1 {
		quickSort(nums[right+1: ])
	}
}
```

## Reference

1. [Wikipedia: Quick sort](https://en.wikipedia.org/wiki/Quicksort)
1. [cnblogs:十大经典排序算法（动图演示）](https://www.cnblogs.com/onepixel/p/7674659.html)