## What is mutex

In computer programming, a mutual exclusion object (mutex) is **a program object that allows multiple program threads to share the same resource**, such as file access, but not simultaneously. When a program is started, a mutex is created with a unique name. After this stage, any thread that needs the resource must lock the mutex from other threads while it is using the resource. The mutex is set to unlock when the data is no longer needed or the routine is finished.

## Mutex in Linux

### What are mutexes?

In the Linux kernel, mutexes refer to **a particular locking primitive that enforces serialization on shared memory systems**, and not only to the generic term referring to 'mutual exclusion' found in academia or similar theoretical text books. Mutexes are sleeping locks which behave similarly to binary semaphores, and were introduced in 2006 as an alternative to these. This new data structure provided a number of advantages, including simpler interfaces, and at that time smaller code ([see Disavantage](http://lwn.net/Articles/164802/))

### Implementation

Mutexes are represented by ‘**struct mutex**’, defined in include/linux/mutex.h and implemented in kernel/locking/mutex.c. These locks use an **atomic variable** (->owner) to keep track of the lock state during its lifetime. Field owner actually contains struct **task\_struct** \* to the current lock owner and it is therefore **NULL** if not currently owned. Since task\_struct pointers are aligned at at least L1\_CACHE\_BYTES, low bits (3) are used to store extra state (e.g., if waiter list is non-empty). In its most basic form it also includes a **wait-queue** and a spinlock that serializes access to it. Furthermore, CONFIG\_MUTEX\_SPIN\_ON_OWNER=y systems use a spinner MCS lock (->osq), described below in (ii).

### When acquiring a mutex, there are three possible paths that can be taken, depending on the state of the lock:

1. **fastpath**: tries to atomically acquire the lock by cmpxchg()ing the owner with the current task. This only works in the uncontended case (cmpxchg() checks against 0UL, so all 3 state bits above have to be 0). If the lock is contended it goes to the next possible path.
2. **midpath**: aka **optimistic spinning**, tries to spin for acquisition while the lock owner is running and there are no other tasks ready to run that have higher priority (need_resched). The rationale is that if the lock owner is running, it is likely to release the lock soon. The mutex spinners are queued up using MCS lock so that only one spinner can compete for the mutex. The MCS lock (proposed by Mellor-Crummey and Scott) is a simple spinlock with the desirable properties of being fair and with each cpu trying to acquire the lock spinning on a local variable. It avoids expensive cacheline bouncing that common test-and-set spinlock implementations incur. An MCS-like lock is specially tailored for optimistic spinning for sleeping lock implementation. An important feature of the customized MCS lock is that it has the extra property that spinners are able to exit the MCS spinlock queue when they need to reschedule. This further helps avoid situations where MCS spinners that need to reschedule would continue waiting to spin on mutex owner, only to go directly to slowpath upon obtaining the MCS lock.
1. **slowpath**: last resort, if the lock is still unable to be acquired, the task is added to the wait-queue and sleeps until woken up by the unlock path. Under normal circumstances it blocks as TASK_UNINTERRUPTIBLE.

While formally kernel mutexes are sleepable locks, it is path (ii) that makes them more practically a hybrid type. By simply not interrupting a task and busy-waiting for a few cycles instead of immediately sleeping, the performance of this lock has been seen to significantly improve a number of workloads. Note that this technique is also used for rw-semaphores.

### Disadvantages

Unlike its original design and purpose, ‘struct mutex’ is among the largest locks in the kernel. E.g: on x86-64 it is 32 bytes, where ‘struct semaphore’ is 24 bytes and rw_semaphore is 40 bytes. Larger structure sizes mean more CPU cache and memory footprint.

### When to use mutexes

Unless the strict semantics of mutexes are unsuitable and/or the critical region prevents the lock from being shared, always prefer them to any other locking primitive.

## Reference

1. [webopedia: mutex - mutual exclusion object](https://www.webopedia.com/TERM/M/mutex.html)
2. [kernel.org: Generic Mutex Subsystem](https://www.kernel.org/doc/html/latest/locking/mutex-design.html)