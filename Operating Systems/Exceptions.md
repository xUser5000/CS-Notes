# Explanation
- An *Exception* is a transfer of control to the OS kernel in response to some event.
	- Illustration: ![[Exception Control flow.png]]
	- Examples: divide by zero, arithmetic overflow, page fault, IO request completes, typing Ctrl+C.
- An *Exception Table* is an array of pointers in the kernel's address space, where the pointer at index k references a function that gets executed if exception number k is triggered.
	- These functions are called *exception handlers*.
	- Illustration: ![[Exception Tables.png]]
- Types of exceptions:
	- *Asynchronous Exception*: an exception that is caused by events external to the CPU.
		- Examples:
			- #timer_interrupt : the kernel takes back control of the current process every few milliseconds.
			- I/O interrupt from an external device: e.g, arrival of a disk page or a network packet.
	- *Synchronous Exceptions*: an exception that is caused as a result of executing an instruction.
		- Examples:
			- *Traps*: e.g, #system_call and breakpoints
			- *Faults*: e.g, page faults and floating-point exceptions
			- *Aborts*: e.g, illegal instruction

# Sources
- [CMU intro to Computer Systems: Lecture 14](https://scs.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=d2759175-d59e-4f80-ab9e-24c2f15c8adb)