# Explanation

## Structure
- Linux virtual address space consists of:
	- User portion: program code, stack, heap, etc...
	- Kernel Portion:
		- Logical addresses
			- This is the normal virtual address space of the kernel.
			- Contains kernel data structures like page tables and per-process kernel stack.
			- Cannot be swapped to disk.
			- Maps directly to the upper portion of physical memory.
				- Makes it simple to translate kernel virtual addresses.
				- Suitable for operations that require contiguous physical memory to work.
					- e.g, Direct Memory Access (DMA).
		- Virtual addresses
			- Not necessarily contiguous in physical memory.
			- Used for large buffers where finding a contiguous large chunk of memory is difficult.
- Kernel pages live in the bottom part of the address space.

## Illustration
![[Linux Address Space.png]]

## Mechanism
- On #context_switch, the user portion of the currently running address space changes while the kernel portion stays the same.
- A [[Process]] running in #user_mode cannot access kernel virtual pages.

# Sources
- OSTEP Chapter 23 - "Complete Virtual Memory Systems"