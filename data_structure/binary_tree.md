# binary tree

## Property

1. For any non-empty binary tree T, if n0 is the no. of leaf nodes & n2 is the no. of nodes of degree 2, then n0 = n2 + 1.
```
Proof:Let n be the total no. of nodes in binary tree.

            n0 no. of nodes of degree 0

            n1  no. of nodes of degree 1

            n2  no. of nodes of degree 2

                        n = n0 + n1 + n2  -----> (1)

            All the nodes except the root node has a branch coming into it. Let B be the no. of branches in the binary tree.

                        n = B + 1 ----->(2)

                        B = 0.n0 + 1 * n1 + 2*n2 since there would be 0 branches from n0, and 1 branch from n1 nodes and two branches from n2 nodes

                        B = n1 + 2n2    ---->3

            Substituting (3) in (2).

                        n =  n1 + 2n2 + 1  ----> 4.

            Equate 1 & 4

                        n0 + n1 + n2 = n1 + 2n2 + 1

                        n0 = n2 + 1
```

## reference

1. [Algorithms And Data Structures](http://cs-algorithms-and-datastructures.blogspot.com/2012/11/some-basics-about-trees.html)