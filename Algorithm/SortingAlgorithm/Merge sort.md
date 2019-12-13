# Merge sort

## Implementation

![](../../Images/Algorithm/SortingAlgorithm/Merge%20sort.gif)

```
func mergeSort(nums []int) {
	nLen := len(nums)
	for i := 1; i < nLen; i <<= 1 {
		for j := 0; j < nLen; j += i << 1 {
			if j+i > nLen {
				continue
			}
			nums1 := nums[j : j+i]
			var nums2 []int
			if j+i<<1 >= nLen {
				nums2 = nums[j+i:]
			} else {
				nums2 = nums[j+i : j+i<<1]
			}
			_len := len(nums1) + len(nums2)
			var _nums []int
			off1 := 0
			off2 := 0
			for off1 < i && off2 < _len-i {
				if nums1[off1] < nums2[off2] {
					_nums = append(_nums, nums1[off1])
					off1++
				} else {
					_nums = append(_nums, nums2[off2])
					off2++
				}
			}
			if off1 < i {
				_nums = append(_nums, nums1[off1:]...)
			}
			if off2 < _len-i {
				_nums = append(_nums, nums2[off2:]...)
			}
			
			for x := j; x < j+_len; x++ {
				nums[x] = _nums[x-j]
			}
		}
	}
}
```

## Reference

1. [Wikipedia: Merge sort](https://en.wikipedia.org/wiki/Merge_sort)
1. [cnblogs:十大经典排序算法（动图演示）](https://www.cnblogs.com/onepixel/p/7674659.html)