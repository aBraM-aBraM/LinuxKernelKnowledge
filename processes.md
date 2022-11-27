# Processes

<details>
<summary>What is a process?</summary>
A process is an abstraction unit created by the operating system, its main purpose is to abstract a program as if it exists by itself and prevent / arrange outside intervention.
</details>

<details>
<summary>Where can processes be found in Linux</summary>
Processes can be found in "/proc/{pid}" in linux. They are represented using a virtual file system (procfs).
</details>

<details>
<summary>What is a thread>?</summary>
A thread is similar to a process but it shares memory with other threads. Every process is built from a number of threads.


The thread is the actual code executing unit, it has its own stack and register values. The kernel's scheduler (the sub-system that switches between every thread) schedules threads and not processes.
</details>

<details>
<summary>Process vs Thread in Linux</summary>
Both threads and processes in linux are created using the clone() syscall. This syscall lets the caller choose what parts to keep shared and what keep private.


---
Here are some flags for context

* CLONE_FILES - shares the file descriptor table with the parent
* CLONE_VM - shares the address space with the parent
* CLONE_FS - shares the filesystem information (root directory, current directory) with the parent
* CLONE_NEWNS - does not share the mount namespace with the parent
* CLONE_NEWIPC - does not share the IPC namespace (System V IPC objects, POSIX message queues) with the parent
* CLONE_NEWNET - does not share the networking namespaces (network interfaces, routing table) with the parent
</details>

<details>
<summary>What are containers such as docker?</summary>
A container is an abstraction unit that can hold processes, network interfaces etc. and run as a guest of the operating system and use the same kernel. 

Containers are very useful because they are lightweight and allow to check programs and a clean environment without interfeering with the host system and without being interfeered by it.
</details>

<details>
<summary>What allows containers to exist? (What are namespaces)</summary>
Namespaces are practically a struct that contains pointers to system related structs such as IPC, networking, mount, time. This abstract concept as a part of linux allows containers because proccesses have their namespace and they can't really tell if they are a host or a guest.
</details>


<details>
<summary>What is a context switch? How does it work?</summary>
The kernel has a lot of processes that need to be executed simultaneously. In order to achieve this the kernel uses the scheduler which gives each process a time of execution and switches between them using a context switch.

A context switch starts from an interrupt. Firstly, userspace registers are saved to the kernel stack and then, execution is transferred to the thread the scheduler chooses.
</details>

<details>
<summary>What are task states?</summary>
Each thread (task) has a state given by the kernel such as RUNNING, READY, INTERRUPTIBLE, DEAD etc.

the state lets the scheduler calculate which process should get execution time next in the list of waiting processes.
</details>


<details>
<summary>When does a task becomes INTERRUPTIBLE?</summary>
When a task calls syscalls its state can be set to interruptible (for example after requesting to open a file). After which it is added to a waiting queue, the scheduler is called to choose a new task from a ready queue and a context switch occurs. In the given example when the open() syscall will finish execution the task will be woken up - set READY and transferred to the ready queue.
</details>