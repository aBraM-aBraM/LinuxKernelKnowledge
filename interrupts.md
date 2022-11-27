# Interrupts

<summary>Interrupt definition</summary>
A hardware triggered event the stops the ongoing execution and jumps to an interrupt handler.

---
<details>
<summary>Synchronous Interrupt</summary>
An interrupt that is triggered because of the current running program. For example division by zero. 
</details>

<details>
<summary>Asynchronous Interrupt</summary>
An interrupt causes by an external source. For example, a sound devices sends data to its driver
</details>

<details>
<summary>Maskable Interrupt</summary>
Interrupts that can be ignored / postponed. For example, the network interface has received a new packet.
signaled via INT processor pin 
</details>

<details>
<summary>Non Maskable Interrupts</summary>
Interrupts that must be handled immediately. For example, memory corruption.
signaled via NMI processor pin
</details>

---

<details>
<summary>Fault vs Trap</summary>
A fault is reported before the cpu has executed the instruction unlike a trap.
</details>

<details>
<summary>What is an example of a programmed exception?</summary>
Calling interrupts as part of code execution.
</details>

<details>
<summary>What is an example of a programmed trap?</summary>
Software breakpoint (`int 3` in x86).
</details>

<details>
<summary>How are interrupts triggered physically></summary>
The processor has interrupt pins (INTR & NMI). In some embedded devices the interrupt pins can be addressed directly from devices. In modern systems there's a PIC (Programmable Interrupt Controller) that exports interrupt request pins (IRQ) to devices and outputs to the CPU's INTR pin.
</details>

<details>
<summary>How do the PIC and CPU communicate?</summary>

* The PIC receives an interrupt from a peripherial, converts the pin it recieved the interrupt from to a vector, writes it into the cpu and raises an interrupt via the INTR pin.

* The PIC waits for the CPU to acknowledge the interrupt and doesn't send new interrupts until then.

* The CPU sends its acknowledgement and recieves the next interrupt.
</details>

<details>
<summary>How do PICs look like in SMP systems?</summary>
Because SMP systems by design have more than one processor the interrupts should be distributed. In order to solve this every processor has its own local APIC that has processor related interrupts like thermal sensors and timers. In addition theres a global I/O APIC that distribute interrupt requests from external devices to the processors usually with a bus in the middle.
</details>


<details>
<summary>How does an interrupt handler looks? Where is it in memory?</summary>
Interrupt Handlers have their own interrupt descriptor table IDT (how original). Different CPU architectures have their interrupt tables at different places.

For example in x86 they can be anywhere in physical memory, in Linux x86 the first 32 are reserved for exception and no' 128 is used for syscalls. The rest are mostly used for hardware interrupts handlers.
</details>

<details>
<summary>What information does an IDT entry hold? (x86)</summary>
The segment, offset and flags determining the type of interrupt
</details>

<details>
<summary>What are the events the CPU does before executing the interrupt handler?</summary>

* Check the privilege required by the interrupt
* Change to the new privilege stack if required, save the old registers & flags on the new stack
* Save the error code in case of abort
* Start executing the interrupt handler
</details>

<details>
<summary>Linux Interrupt Phases</summary>

* Critical - evaluate the requested interrupt handler and acknowledge the interrupt.
* Immediate - execute the requested interrupt handler (device irq for example)
* Deferred - reenable interrupts and return
</details>

<details>
<summary>Nested Interrupts Linux</summary>

Linux currently offers a strict rules for nesting interrupts.

* Exceptions such as a page fault or system call cannot cause another interrupt.

* Interrupts can cause exceptions

* Interrupts cannot cause interrupts
</details>

<details>
<summary>What is an Interrupt Context? are syscalls in interrupt context?</summary>

An interrupt context is a context of execution that is not directly related to any concept of process and has started due to an IRQ (not exceptions such as syscall).

An interrupt context is not allowed to trigger a context switch meaning - no sleeping no reading user memory etc.
</details>

<details>
<summary>What are deferrable actions? </summary>

Parts of an interrupt handler's logic that is exported out of the interrupt handler and scheduled to avoid expensive interrupt handlers which degrades the system's functioning. Some are done in interrupt context (SoftIRQS) and other in process context.
</details>
