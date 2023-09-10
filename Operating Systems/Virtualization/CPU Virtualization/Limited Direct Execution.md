# Explanation
- *Direct Execution*: running programs directly on the CPU (not in a virtual environment for example).
	- Advantage: fast.
	- Disadvantage: does not support executing restricted operations such as issuing IO requests, reading a file, or gaining access to more RAM.
- *Limited Direct Execution*: same as *Direct Execution* but supports more features.
- Limited direct execution differentiates between two modes of operation:
	- #user_mode: process code that is restricted in what it can do.
	- #kernel_mode: OS code that can run anything on the CPU.
- If a process wants to execute a privileged instruction, it invokes a #system_call.
	- A process must execute a special instruction called `trap` to execute a #system_call.
		- `trap` jumps into the kernel and raises the privilege level to #kernel_mode.
	- The kernel must execute another special instruction called `return-from-trap` that returns back to the process code and reduces the privilege level to #user_mode.
	- The OS stores the current state of the process (e.g, registers, flags, program counter, etc...) in a per-process #kernel_stack to be able to return correctly when executing `return-from-trap`.
- When the machine boots up:
	- It will be in #kernel_mode 
	- The OS sets up a #trap_table that maps every trap number to a #trap_handler location.
- When a process is currently running, the OS is not executing instructions. Thus, it cannot do anything at all.
	- e.g, it cannot do a #context_switch
- There are two approaches for the OS to get back control of the CPU:
	- Cooperative approach:
		- The OS trusts the processes in the system to behave reasonably.
			- It assumes that processes that run for too long periodically gives up the CPU.
		- Systems that follow this style often includes a `yield()` system call, which transfers control back to the OS.
		- Was implemented in the early versions of Macintosh.
		- Disadvantage: a process might run forever and thus not allowing the OS to run at all.
	- Non-cooperative approach (AKA #timer_interrupt): 
		- A timer device can be programmed to raise an interrupt every few milliseconds.
		- When the interrupt is raised, the currently running process is halted and a pre-configured #interrupt_handler in the OS runs.

# FAQ
- How does the OS know which #system_call to execute after a `trap` instruction?
	- The user code must provide a #system_call_number at a specific register/location so that the OS knows which system call to execute.
- Why can't user code just specifies the location in kernel code to execute the system call?
	- Because doing so would allow user programs to jump anywhere into the kernel, which is clearly a very bad idea.
- What happens if the OS receives an interrupt while executing an #interrupt_handler?
	- Most operating systems disable-interrupts while executing an #interrupt_handler.
	- Other systems implement a sophisticated locking mechanisms to make sure nothing goes wrong.

# Sources
- OSTEP Chapter 6 - "Mechanism: Limited Direct Execution"