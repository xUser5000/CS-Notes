# Explanation
- A *process* is an instance of a running program.
- A process provide two main abstractions:
	- A logical control flow:
		- Each program seems to have exclusive access to the CPU.
		- This is provided by kernel context switching.
	- A private address space
		- Each program seems to have exclusive use of main memory.
		- This is provided by virtual memory mechanism.

# Sources
- Extends [[Exceptions#Sources]]