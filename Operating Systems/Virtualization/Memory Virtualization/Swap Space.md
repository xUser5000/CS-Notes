# Explanation
- *Swap Space*: is a reversed space on the disk for moving pages back and forth.
	- The OS can read and write to the swap space in page-sized units.
	- Allows the OS to pretend that memory is larger than it actually is.
- *Present bit*: bit in the page table that indicates whether or not the page is in physical memory.
- *Page Fault*: accessing a page that is not in physical memory.
- *Page-Fault Handler*: a piece of code that runs when a page fault happens to service the memory request.
	- All systems handle page faults in software, even with hardware-managed TLB.
	- When the OS receives a page fault, it looks in the page table to find the address and issues an IO request to the disk to fetch the needed page into memory.
	- Note: while IO is in flight, the process will be in a blocked state.
 - If memory is full, the OS selects some pages in memory to swap out, based on a [[Page-replacement Algorithm]].
	- A background thread (called the Swap Daemon) that swaps out some pages out of memory if the amount of free pages is below a certain threshold.

# FAQ
- Why do we want to support large address space for a process?
	- Ease of use: you don't have to worry about if there is room enough i memory for your program's data structures.
- Why does not hardware handle page faults?
	- The disk read is traditionally more so slow that the extra overhead of running the software is minimal.
	- The hardware would have to understand swap space, how to issue IO disk, and other details.
- When does the OS swap pages out of memory?
	- 

# Sources
- OSTEP Chapter 21 - "Beyond Physical Memory: Mechanisms"