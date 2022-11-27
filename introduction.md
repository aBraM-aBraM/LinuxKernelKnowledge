# Introduction


<details>
<summary>Kernel Space vs User Space</summary>

* Kernel space is reserved to the kernel and It cannot be accessed
from the user space (in x86 it is enforced by the hardware - segment selector).


* User space is a range of memory reserved for a single process
it can be accessed from the kernel directly. One example for this usage is the kernel filling a buffer given from usermode as part of a syscall.
</details>

<details>
<summary>Writing to arbitrary addresses (Kernel vs UserMode)</summary>

* The data will be written. It may lead to a system crash
further down the line but writing to memory from kernel context
is permissable

* The cpu will try to write to the memory. In usermode the memory space has different permissions such as read, write and execute. If the data was written to an area without write permission a page fault will trigger an interrupt which the kernel will handle.
</details>

<details>
<summary>Monolithic Kernel vs Micro Kernel</summary>

* In a monolithic kernel all of the system related logic (drivers, scheduler, filesystem, networking etc.) is located in kernel space. User mode application can access get services from the kernel using syscalls

* In a micro kernel much of the system's logic is transferred to usermode. This design makes much of the kernel's different parts protected from each other as they execute in user space. Only essential parts stay in kernel space such as the scheduler, ipc etc.
</details>

<details>
<summary>Virtual Address Space: Kernel vs UserMode</summary>
In typical implementations the kernel resides in the top of the virtual memory whilst the user mode at the bottom.

>The kernel has its own stack!
</details>

<details>
<summary>ASMP vs SMP</summary>

* ASMP uses only one CPU core for the kernel and the rest of usermode. This design of course is not very scalable.
* SMP uses all cores for kernel and usermode logic. This design is more scalable but much harder to implement because a lot of race conditions need to be avoided using synchronization primitives.
</details>

<details>
<summary>Portable Development</summary>
In order to write portable code the amount of cpu specific code
should be minimized. In the linux kernel this is done in the "arch" directory.
</details>

<details>
<summary>What's a device driver (driver)</summary>
A driver is a program that runs in kernel mode. Its purpose is to communicate with a physical device and export services for the kernel or usermode applications to use this device as intended.
</details>