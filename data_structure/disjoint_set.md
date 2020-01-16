# Disjoint-set data structure

In computer science, a disjoint-set data structure (also called a union–find data structure or merge–find set) is a data structure that tracks a set of elements partitioned into a number of disjoint (non-overlapping) subsets. It provides near-constant-time operations (bounded by the inverse Ackermann function) to add new sets, to merge existing sets, and to determine whether elements are in the same set. In addition to many other uses (see the Applications section), **disjoint-sets play a key role in Kruskal's algorithm for finding the minimum spanning tree of a graph**.

## Time complexity

Without path compression (or a variant), union by rank, or union by size, the height of trees can grow unchecked as O(n), implying that Find and Union operations will take O(n) time.

Using path compression alone gives a worst-case running time of n+f*(1+ log(2+f/n) n) , for a sequence of n MakeSet operations (and hence at most n-1 Union operations) and f Find operations.

Using union by rank alone gives a running-time of O(m log2 n) (tight bound) for m operations of any sort of which n are MakeSet operations.

Using both path compression, splitting, or halving and union by rank or size ensures that the amortized time per operation is only O(alpha(n)), which is optimal, where alpha(n) is the inverse Ackermann function. This function has a value alpha(n)<5 for any value of n that can be written in this physical universe, so the disjoint-set operations take place in essentially constant time.

## Implementation

### MakeSet

The MakeSet operation makes a new set by creating a new element with a unique id, a rank of 0, and a parent pointer to itself. The parent pointer to itself indicates that the element is the representative member of its own set.

The MakeSet operation has O(1) time complexity, so initializing n sets has O(n) time complexity.

### Find

Find(x) follows the chain of parent pointers from x up the tree until it reaches a root element, whose parent is itself. This root element is the representative member of the set to which x belongs, and may be x itself.

#### Path compression

Path compression flattens the structure of the tree by making every node point to the root whenever Find is used on it. This is valid, since each element visited on the way to a root is part of the same set. The resulting flatter tree speeds up future operations not only on these elements, but also on those referencing them.

Tarjan and Van Leeuwen also developed one-pass Find algorithms that are more efficient in practice while retaining the same worst-case complexity: path splitting and path halving.

#### Path halving

Path halving makes every other node on the path point to its grandparent.

#### Path splitting

Path splitting makes every node on the path point to its grandparent.

### Union

Union(x,y) uses Find to determine the roots of the trees x and y belong to. If the roots are distinct, the trees are combined by attaching the root of one to the root of the other. If this is done naively, such as by always making x a child of y, the height of the trees can grow as O(n). To prevent this union by rank or union by size is used.

#### by rank

Union by rank always attaches the shorter tree to the root of the taller tree. Thus, the resulting tree is no taller than the originals unless they were of equal height, in which case the resulting tree is taller by one node.

To implement union by rank, each element is associated with a rank. Initially a set has one element and a rank of zero. If two sets are unioned and have the same rank, the resulting set's rank is one larger; otherwise, if two sets are unioned and have different ranks, the resulting set's rank is the larger of the two. Ranks are used instead of height or depth because path compression will change the trees' heights over time.

#### by size

Union by size always attaches the tree with fewer elements to the root of the tree having more elements.

## Codes

```
type DisjointSet struct {
	ranks []int
	subsets []int
}

func (d *DisjointSet) Constructor (cnt int) {
	d.ranks = make([]int, cnt)
	d.subsets = make([]int, cnt)
	for i := 0; i < cnt; i++ {
		d.ranks[i] = 1
		d.subsets[i] = i
	}
}

func (d *DisjointSet) Find(i int) int {
	if d.subsets[i] != i {
		d.subsets[i] = d.Find(d.subsets[i])
	}
	return d.subsets[i]
}

func (d *DisjointSet) Union(i, j int) {
	rooti := d.Find(i)
	rootj := d.Find(j)

	if rooti == rootj {
		return
	}

	if d.ranks[rooti] <= d.ranks[rootj] {
		d.subsets[rooti] = rootj
		if d.ranks[rooti] == d.ranks[rootj] {
			d.ranks[rootj]++
		}
	} else if d.ranks[rooti] > d.ranks[rootj] {
		d.subsets[rootj] = rooti
	}
}
```

## Reference

1. [Wikipedia: Disjoint-set data structure](https://en.wikipedia.org/wiki/Disjoint-set_data_structure#Time_complexity)
1. [GeeksforGeeks: Disjoint Set (Or Union-Find) | Set 1 (Detect Cycle in an Undirected Graph))](https://www.geeksforgeeks.org/union-find/)
1. [GeeksforGeeks: Union-Find Algorithm | Set 2 (Union By Rank and Path Compression)](https://www.geeksforgeeks.org/union-find-algorithm-set-2-union-by-rank/)