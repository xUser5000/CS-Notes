# Explanation
- A program address space generally consists of the following segments: [1]
	- #call_stack 
	- The Heap segment: contains data that is dynamically allocated using `malloc()`, `calloc()`, `free()`.
	- The Data segment: contains statically allocated data (e.g, global variables, static variables, and string constants).
	- Text / Shared libraries
		- contains read-only executable machine instructions.
		- Usually, it's shareable so that multiples instances of the same program can be run without needing to duplicate its code segment.

# FAQ
- Why is the default #call_stack size limited? [2]
	- To support creation of a large number of threads in multi-threaded programs.
	- To fit entirely in the CPU's cache.
		- Allocating large objects on the #call_stack would make it impossible on most machines to have them loaded at once into cache, which pretty much defeats the purpose of the #call_stack.
	- It's an error detection and containment mechanism.
		1. If the #call_stack grows out of bounds (e.g. a lot of recursive calls), it is almost always an error in the design and/or the behavior of the application.
		2. If case mentioned in 2 happens, errors (like infinite recursion) would be caught very late, only after the operating systems resources are exhausted.

# Sources
1. [CMU Introduction to Computer Systems - Lecture 9](https://scs.hosted.panopto.com/Panopto/Pages/Sessions/List.aspx#folderID=%22b96d90ae-9871-4fae-91e2-b1627b43e25e%22)
2. [Stack Overflow - Why is stack memory size so limited](https://stackoverflow.com/questions/10482974/why-is-stack-memory-size-so-limited)
