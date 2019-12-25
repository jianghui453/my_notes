# defer()

## Important Points

1. In Go language, multiple defer statements are allowed in the same program and they are executed in LIFO(Last-In, First-Out) order as shown in Example 2.
1. In the defer statements, the arguments are evaluated when the defer statement executed, not when they called.
1. Defer statements are generally used to ensure that the files are closed when your work is finished with them, or to close the channel, or to catch the panics in the program.

## Rederence

1. [GeeksforGeeks: Defer Keyword in Golang](https://www.geeksforgeeks.org/defer-keyword-in-golang/)