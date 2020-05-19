# defer()

## Important Points

1. In Go language, multiple defer statements are allowed in the same program and they are executed in LIFO(Last-In, First-Out) order as shown in Example 2.
1. In the defer statements, the arguments are evaluated when the defer statement executed, not when they called.
1. Defer statements are generally used to ensure that the files are closed when your work is finished with them, or to close the channel, or to catch the panics in the program.

## 知识点

1. 多个 defer 的执行顺序是一个栈，先入后出；
2. 方法中出现 panic 仍然会执行本函数中的 defer ；
3. 传入 defer 的参数会在声明时就计算好；

## 引用

1. [GeeksforGeeks: Defer Keyword in Golang](https://www.geeksforgeeks.org/defer-keyword-in-golang/)