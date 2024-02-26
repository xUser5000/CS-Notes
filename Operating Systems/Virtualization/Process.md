# Explanation
- A *process* is an instance of a running program.
- A *program* is just some silent bits that sit on the disk.
- A process provides two main abstractions:
	- A logical control flow:
		- Each program seems to have exclusive access to the CPU.
		- This is provided by [[CPU Virtualization]].
	- A private address space
		- Each program seems to have exclusive use of main memory.
		- This is provided by [[Memory Virtualization]].
- To run a program, the OS must:
	1. load the code and static data into memory (refer to [[Memory Layout]])
		- This is usually done lazily (known as #demand_paging), where the OS does not actually load the disk pages until they are needed.
	2. Allocate memory for the #call_stack and heap (refer to [[Memory Layout]])
	3. Fill in the values of `argc` and `argv`
	4. Open three #file_descriptor s for the newly-created-process (`stdin`, `stdout`, and `stderr`)
	5. Finally, jump to the `main()` routine 
- Process states:
	- *Running*: the process is currently executing instructions on the CPU
	- *Ready*: the process is ready to run but the OS hasn't chosen it to run yet (probably due to the scheduling algorithm).
	- *Blocked*: the process cannot be run at the moment.
		- e.g, issuing an IO request and waiting for the result
- The OS keeps track of all running processes in the systems as well as their states in a data structure called the #process_list (AKA #process_control_block).

# Sources
- Extends [[Exceptions#Sources]]
- OSTEP Chapter 4 - "The Abstraction: The Process"
