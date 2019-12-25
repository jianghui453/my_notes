# Morris Traversal Preorder

## Implementation

Using Morris Traversal, we can traverse the tree without using stack and recursion. The algorithm for Preorder is almost similar to Morris traversal for Inorder.

- If left child is null, print the current node data. Move to right child.
- Else, Make the right child of the inorder predecessor point to the current node. Two cases arise:
    - The right child of the inorder predecessor already points to the current node. Set right child to NULL. Move to right child of current node.
    - The right child is NULL. Set it to current node. Print current node’s data and move to left child of current node.
- Iterate until current node is not NULL.

## Reference

1. [GeeksforGeeks: Morris traversal for Preorder](https://www.geeksforgeeks.org/morris-traversal-for-preorder/)