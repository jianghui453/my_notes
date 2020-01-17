# User mode and Kernel mode

用户态和内核态的主要区别就是权限不同，内核态拥有所有权限，而用户态权限受限。如果在内核态出现错误会导致整个系统的崩溃，而在用户态则是抛出异常或者结束本进程，不会影响到其他进程的运行。保护了系统的稳定性。

## Kernel Mode

In Kernel mode, the executing code has complete and unrestricted access to the underlying hardware. It can execute any CPU instruction and reference any memory address. Kernel mode is generally reserved for the lowest-level, most trusted functions of the operating system. Crashes in kernel mode are catastrophic; they will halt the entire PC.

## User Mode

In User mode, the executing code has no ability to directly access hardware or reference memory. Code running in user mode must delegate to system APIs to access hardware or memory. Due to the protection afforded by this sort of isolation, crashes in user mode are always recoverable. Most of the code running on your computer will execute in user mode.