# Skip list

See the following example list with 16 nodes and two layers. The upper layer works as an “express lane” which connects only main outer stations, and the lower layer works as a “normal lane” which connects every station. Suppose we want to search for 50, we start from first node of “express lane” and keep moving on “express lane” till we find a node whose next is greater than 50. Once we find such a node (30 is the node in following example) on “express lane”, we move to “normal lane” using pointer from this node, and linearly search for 50 on “normal lane”. In following example, we start from 30 on “normal lane” and with linear search, we find 50.

![](../Images/DataStructures/SkipList/1.png)

## What is the time complexity with two layers?

The worst case time complexity is number of nodes on “express lane” plus number of nodes in a segment (A segment is number of “normal lane” nodes between two “express lane” nodes) of “normal lane”. So if we have n nodes on “normal lane”, √n (square root of n) nodes on “express lane” and we equally divide the “normal lane”, then there will be √n nodes in every segment of “normal lane” . √n is actually optimal division with two layers. With this arrangement, the number of nodes traversed for a search will be O(√n). Therefore, with O(√n) extra space, we are able to reduce the time complexity to O(√n).

## Insertion

Each element in the list is represented by a node, the level of the node is chosen randomly while insertion in the list. Level does not depend on the number of elements in the node. The level for node is decided by the following algorithm

```
randomLevel()
lvl := 1
//random() that returns a random value in [0...1)
while random() < p and lvl < MaxLevel do
lvl := lvl + 1
return lvl
```

Each node carries a key and a forward array carrying pointers to nodes of a different level. A level i node carries i forward pointers indexed through 0 to i.

![](../Images/DataStructures/SkipList/2.jpg)

We will start from highest level in the list and compare key of next node of the current node with the key to be inserted. Basic idea is If –

1. Key of next node is less than key to be inserted then we keep on moving forward on the same level.
1. Key of next node is greater than the key to be inserted then we store the pointer to current node i at update and move one level down and continue our search.

![](../Images/DataStructures/SkipList/3.jpg)


## Code

```
// 跳表

package linked_list

import (
	// "fmt"
	rand "math/rand"
	// "math"
)

type SkipListNode struct {
	Key     int
	Forword []*SkipListNode
}

func NewSkipListNode(key, level int) *SkipListNode {
	n := new(SkipListNode)
	n.Key = key
	n.Forword = make([]*SkipListNode, level+1)
	return n
}

type SkipList struct {
	Header   *SkipListNode
	MaxLevel int
	Level    int
	P        float64
}

func NewSkipList(maxLevel int, p float64) *SkipList {
	s := new(SkipList)
	s.MaxLevel = maxLevel
	s.P = p
	return s
}

func (s *SkipList) Insert(key int) {
	cur := s.Header

	if cur == nil {
		s.Header = NewSkipListNode(key, 0)
	} else {
		rLevel := s.genRandomLevel()
		n := NewSkipListNode(key, rLevel)
		if s.Level < rLevel {
			s.Header.Forword = append(s.Header.Forword, make([]*SkipListNode, rLevel-s.Level)...)
			s.Level = rLevel
		}

		nodes := make([]*SkipListNode, s.Level+1)
		for i := s.Level; i >= 0; i-- {
			for cur.Forword[i] != nil && cur.Forword[i].Key < key {
				cur = cur.Forword[i]
			}
			nodes[i] = cur
		}

		if cur.Key == key {
			return
		}

		for i := 0; i <= rLevel; i++ {
			n.Forword[i], nodes[i].Forword[i] = nodes[i].Forword[i], n
		}
	}
}

func (s *SkipList) Search(key int) *SkipListNode {
	cur := s.Header
	for i := s.Level; i >= 0; i-- {
		for cur.Forword[i] != nil && cur.Forword[i].Key <= key {
			cur = cur.Forword[i]
		}
	}

	if cur.Key == key {
		return cur
	}

	return nil
}

func (s *SkipList) Delete(key int) {
	if s.Header.Key == key {
		header := s.Header.Forword[0]

		l := len(header.Forword)
		if l < s.Level {
			header.Forword = append(header.Forword, make([]*SkipListNode, s.Level-l+1)...)
		}

		for i := s.Level ; i >= 0; i-- {
			if s.Header.Forword[i] == header && header.Forword[i] == nil {
				header.Forword = header.Forword[: i]
			} else {
				for ; i >= 0; i-- {
					if header.Forword[i] == nil {
						header.Forword[i] = s.Header.Forword[i]
					}
				}
			}
		}

		s.Header = header
		s.Level = len(header.Forword) - 1
	} else {
		cur := s.Header
		nodes := make([]*SkipListNode, s.Level+1)
		for i := s.Level; i >= 0; i-- {
			for cur.Forword[i] != nil && cur.Forword[i].Key < key {
				cur = cur.Forword[i]
			}
			nodes[i] = cur
		}

		cur = cur.Forword[0]
		if cur.Key > key {
			return
		}

		l := len(cur.Forword)
		for i := l - 1; i >= 0; i-- {
			if i > 0 && cur.Forword[i] == nil {
				s.Header.Forword = s.Header.Forword[: i]
			} else {
				nodes[i].Forword[i] = cur.Forword[i]
			}
		}
		s.Level = len(s.Header.Forword)-1
	}
}

func (s *SkipList) genRandomLevel() int {
	var (
		i      int     = 0
		random float64 = rand.Float64()
	)

	for i < s.MaxLevel && s.P < random {
		i, random = i+1, rand.Float64()
	}
	return i
}
```

## Reference

1. [GeeksforGeeks: Skip List | Set 1 (Introduction)](https://www.geeksforgeeks.org/skip-list/)
1. [GeeksforGeeks: Skip List | Set 2 (Insertion)](https://www.geeksforgeeks.org/skip-list-set-2-insertion/)
1. [GeeksforGeeks: Skip List | Set 3 (Searching and Deletion)](https://www.geeksforgeeks.org/skip-list-set-3-searching-deletion/)