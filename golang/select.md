# select

In Go language, the select statement is just like switch statement, but in the select statement, case statement refers to communication, i.e. sent or receive operation on the channel.

## Syntax

```
select{

case SendOrReceive1: // Statement
case SendOrReceive2: // Statement
case SendOrReceive3: // Statement
.......
default: // Statement
```

## Important points

1. Select statement waits until the communication(send or receive operation) is prepared for some cases to begin.
1. If a select statement does not contain any case statement, then that select statement waits forever.
1. The default statement in the select statement is used to protect select statement from blocking. This statement executes when there is no case statement is ready to proceed.
1. The blocking of select statement means when there is no case statement is ready and the select statement does not contain any default statement, then the select statement block until at least one case statement or communication can proceed.
1. In select statement, if multiple cases are ready to proceed, then one of them can be selected randomly.

## Reference

1. [GeeksforGeeks: Select Statement in Go Language](https://www.geeksforgeeks.org/select-statement-in-go-language/)