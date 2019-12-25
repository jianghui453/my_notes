# Bucket sort

Bucket sort, or bin sort, is a sorting algorithm that works by distributing the elements of an array into a number of buckets. Each bucket is then sorted individually, either using a different sorting algorithm, or by recursively applying the bucket sorting algorithm. It is a distribution sort, a generalization of pigeonhole sort, and is a cousin of radix sort in the most-to-least significant digit flavor. Bucket sort can be implemented with comparisons and therefore can also be considered a comparison sort algorithm. The computational complexity depends on the algorithm used to sort each bucket, the number of buckets to use, and whether the input is uniformly distributed.

## Implementation

![](../../Images/Algorithm/SortingAlgorithm/Bucket%20sort.png)

```
import (
	gosort "sort"
)

func bucketSort(nums []float64) {
	l := len(nums)
	bucket := make([][]float64, 10)

	for i := 0; i < l; i++ {
		bucket[int(nums[i]*10)] = append(bucket[int(nums[i]*10)], nums[i])
	}

	k := 0
	for i := 0; i < 10; i++ {
		gosort.Float64s(bucket[i])
		for j := range bucket[i] {
			nums[k] = bucket[i][j]
			k++
		}
	}
}
```

## Reference

1. [Wikipedia: Bucket sort](https://en.wikipedia.org/wiki/Bucket_sort)
1. [cnblogs:十大经典排序算法（动图演示）](https://www.cnblogs.com/onepixel/p/7674659.html)