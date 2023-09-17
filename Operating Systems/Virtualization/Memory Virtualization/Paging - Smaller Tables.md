# Explanation
- Simple array-based page tables take up too much space.

# Solution 1: Bigger Pages
- Explanation: Bigger pages lead to smaller page tables.
- Disadvantage: #internal_fragmentation.

# Solution 2: Paging and Segments
- Explanation:
	- we can have a page table per logical segment, instead of having a single page table for the entire address space.
	- We use [[Base and Bounds]] register to point to the start of the physical address of the page table for each segment.
- Disadvantages:
	- Extends [[Segmentation#Disadvantages]]

# Solution 3: Multi-level Page Tables
- Explanation:
	- Turns the linear page table to something like a tree.
		- In some cases, a deeper tree is needed (more than two levels).
- Mechanism:
	- Divide the page table into page-sized units.
	- If an entire page of the page table is invalid, don't allocate that page of the page table at all.
	- *Page Directory*: a data structure that serves as a level of indirection above the page table.
		- Tracks whether a page of the page table is valid or not.
		- Tracks the location of each page of the page table (if valid).
- Illustration: ![[Multi-level Page Table.png]]
- Advantages:
	- Allocates page-table space in proportion to the amount of address space you are using
	- Compact
	- Supports sparse address spaces
	- Simple memory management: each portion of the page table can fit neatly within the size of a page.
- Disadvantages:
	- On a TLB miss, two loads from memory will be required.
	- Handling a page table lookup is more complex than using a simple-linear page table.

# Sources
- OSTEP Chapter 20 - "Paging: Smaller Tables"