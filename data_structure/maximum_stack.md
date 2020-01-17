Below is the step by step algorithm to do this:

1. Create an auxiliary stack, say ‘trackStack’ to keep the track of maximum element
1. Push the first element to both mainStack and the trackStack.
1. Now from the second element, push the element to the main stack. Compare the element with the top element of the track stack, if the current element is greater than top of trackStack then push the current element to trackStack otherwise push the top element of trackStack again into it.
1. If we pop an element from the main stack, then pop an element from the trackStack as well.
1. Now to compute the maximum of the main stack at any point, we can simply print the top element of Track stack.

## 引用

1. [GeeksforGeeks: Tracking current Maximum Element in a Stack](https://www.geeksforgeeks.org/tracking-current-maximum-element-in-a-stack/)