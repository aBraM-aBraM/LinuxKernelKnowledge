# Symmetric Multi-Processing (SMP)

<details>

<summary>What is an atomic operation? How can we make such operations</summary>

An instruction that cannot be interrupted by an interrupt.
We can do such operations by using hardware specific operations or by using cross platform wrappers. Linux offers atomic operation functions for example.

In x86 this happens by using the lock keyword in assembly which makes sure the used resource is owned by the instructions.
</details>

<details>
<summary>What is the purpose of disabling preemption (interrupts)</summary>
On a single core systems the only sense of concurrency is interrupts thus by disabling interrupts there's no concurrency and there's no need for synchronization
</details>

<details>
<summary>What is the purpose of spinlocks?</summary>
On multi core systems disabling interrupts is not enough as two cores can have race conditions on the same variable in memory. In order to solve this spinlocks are used. 

One core acquires a spinlock and when the other core tries to access the variable it firsts tries to acquire the lock thus looping ("spinning") until it is freed. When the first core finishes accessing the data it frees the lock.
</details>

<details>
<summary>What kind of CPU caches exist?</summary>

Caches exist to improve CPUs performance. 

* L1 - local cache, only available to one core
* L2 - shared between all cores
</details>

<details>
<summary>How is cache coherency preserved?</summary>
There are two main protocols:

* Bus snoofing - all memory bus transactions are monitored with caches and when uncoherency is detected the monitor takes action.

* Directory based - a unique "directory" entity exists to validate coherency. cache communicate directly with the directory to preserve the coherency.
</details>

<details>
<summary>What is one drawback of spinlocks? How was it improved?</summary>

The simple implementation of spinlocks checks the lock's value using the lock assembly keyword, as most of the time the lock is locked in you can have 3 processors checking the memory of the lock a lot of times (locking and unlocking the address at the cache each time). This causes cache thrashing (the value gets corrupted).

In order to improve this design spinlocks can first have a normal check on the lock and only after the first check if confirmed use the atomic lock keyword - removing the atomic instruction in most cases and reducing cache thrasing.

In modern operating systems locks are created in lists in memory thus reducing cache thrashing by a lot due to the distance between each lock.
</details>

---
In a (common) scenario where both a process and interrupt context access shared data how can synchronization be assured?

On single core systems we can do this by disabling interrupts, but that won't work on multi-core systems, as we can have the process running on one CPU core and the interrupt context running on a different CPU core.

<details>
<summary>Why a spinlock will cause a deadlock in this scenario?</summary>

* The process locks the object
* The same CPU interrupts and tries to acquire the lock in interrupt context (starts spinlocking)
* Deadlock ^.^
</details>

<details>
<summary>How is this problem solved?</summary>

* The process firsts disables preempting (interrupts) then acquires the spinlock
* Interrupt context always acquires the spinlock 

The difference is that in this situation no interrupt can be raised from the process CPU thus we avoid the deadlock
</details>

<details>
<summary>What is a mutex? How does it differ from a spinlock?</summary>
A mutex is a lock object that is only available in process context. Behind the scenes it uses an optimized version of a spinlock. The operating system gives the mutex to the process and this way enables a high level way of synchronization.
</details>

<details>
<summary>What are the CPU's read, write and simple barriers and why are they needed?</summary>

Today compilers and CPU work together to utilize the fact that a CPU can separate some operations to smaller operations and this way parallelize two operations. 

In order to synchronize this parallelism barriers are used (most of the times they aren't needed but they exists)

* Read barrier - all operations before the barrier would complete their reads until that barrier
* Write barrier - all operations before the barrier would complete their writes until that barrier
* Simple barrier - Read barrier + Write barrier
</details>

---

Read Copy Update - an improvement of read, write barriers for removing elements from lists. doesn't requires locks for reads still requires locks for writes to avoid races between writers.

This mechanism splits removing an element to the removal and elimination. After removal the element still exists as others may still have references to it. The elemination and real removal is postponed until other readers finish traversal on the iterable.