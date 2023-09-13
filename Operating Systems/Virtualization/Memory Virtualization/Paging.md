# Explanation
- *Paging*: dividing virtual space into fixed-size chucks (pages) and mapping each chuck to a place in physical memory.
- Views physical memory as an array of fixed-size slots called *Page Frames*.
- *Page Table*: a **per-process** data structure that stores where each virtual page of the address space lives in physical memory.
	- Stored in main memory because they can get very large.
	- Simplest form: *Linear Page Table*, which is basically an array the OS can index into.
- *Virtual Page Number* (VPN): a unique index of each page in the virtual address space.
- *Physical Frame Number* (PFN): same as VPN but for pages in physical memory.
- Contents of the page table entry (PTE):
	- *Valid bit*: indicates if page is valid.
		- e.g, All unused space between the stack and the heap are invalid.
	- *Protection bits*: indicates if the page can be read, written, or executed.
	- *Present bits*: indicates if this page is in physical memory or in disk and should be swapped in.
	- *Reference bit*: track whether this page has been accessed or not.
		- Useful for page replacement algorithms.
- x86 Page Table Entry (PTE): ![[x86 Page Table Entry (PTE).png]]

## Mechanism
1. ![[Address Translation in Paging.png]]
2. ![[Accessing Memory With Paging - Sample Code.png]]

## Hardware Support
- Extends [[Segmentation#Hardware Support]]
- Stores a single *page-table base register* that contains the physical address of the starting location of the page table.

## Operating System Support
- Extends [[Segmentation#Operating System Support]]
- To do a #context_switch, the OS has to manage a different page table for it.

# Advantages
- Extends [[Segmentation#Advantages]]
- Doesn't lead to external fragmentation. 
- Allows the pages in virtual address to be placed at different locations in physical memory.
- Flexible enough to support the abstraction of an address space regardless of how the process uses uses it.
	- e.g, doesn't make assumptions about the directions in which the heap and stack grow or how they are used.
- Simple in managing free space since all pages are of the same size.

# Disadvantages
- Too slow because it requires an extra memory reference to the page table.
- Wastes a lot of memory just for storing the page table.
	- For a 32-bit address space with 4KB pages, 4 bytes per PTE, and 100 running processes, it takes ~400 MB to store all address translations.

# Sources
- OSTEP Chapter 18 - "Paging: Introduction"