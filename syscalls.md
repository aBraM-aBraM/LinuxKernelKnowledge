# System Calls

<details>
<summary>Syscall definition</summary>
A service offered by the kernel, resembles a function call.
</details>

<details>
<summary>Why do syscalls run in kernel mode?</summary>

Syscalls by definition are services offered by the kernel. They run in kernel mode to retrieve data to user mode in a safe manner.
</details>

<details>
<summary>How does a syscall looks in assembly?</summary>

A syscall looks similar to a function call. It passes arguments and calls an interrupt handler (modern systems even have a syscall cpu instruction). 
</details>

<details>
<summary>What happens after the syscall was called?</summary>

* The syscall interrupt handler is trigged

* A kernel mode switch occurs - cpu switches to kernel stack and the usermode stack is copied to the kernel stack.

* The syscall is identified by an index given with the syscall used in the syscall table. the request system call is executed with the given parameters.

* user registers are restored an execution switch back to user and the user execution continues from the point it left.
</details>

<details>
<summary>If kernel space and user space is different how can functions pass pointers to syscalls?</summary>

The kernel has a set of macros used to translate usermode pointers to their address in kernel mode. 
</details>

<details>
<summary>What is a vsyscall</summary>

vsyscall is an optimization to a syscall that can be used some of the time. Calling a system call is quite a process (switching to kernel mode). Some system calls are very simple like gettimeofday for example. Using vsyscall the usermode can access a VDSO (virtual dynamic shared object) page that is updated by the kernel accordingly thus the syscall is avoided.
</details>
