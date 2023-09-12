# Explanation
- Instead of having a single [[Base and Bounds]] pair in the MMU, we can have a base and bounds pair per logical segment of the address space.
- *Segment*: a contiguous portion of the address space of a particular length. (see [[Memory Layout]])
	- e.g, code, stack, and heap

## Mechanism
1. On a memory reference, the MMU takes the virtual address and determines in which segment it resides. There are two approaches to know the segment of a given virtual address:
	- *Explicit*: chops up the address space into segments based on the top few bits of the virtual address.
		- Illustration: ![[Segmentation Sample Code.png]]
	- *Implicit*: determines the segment by noticing how the address was formed. For example:
		- If the address was generated from the program counter, then it's a reference to the code segment.
		- If the address is based off of the stack, then it must be a reference to the stack segment.
		- Any other address must be in the heap.
2. It extracts the offset and adds it to the base value of the segment to arrive at the desired physical location.

## Hardware Support
- Extends [[Base and Bounds#Hardware Support]]
- If the offset exceeds the size of the segment, the hardware raises a #segmentation_fault.
- Account for segments that grow backward (such as the stack).
	- It stores and extra bit for each segment to tell the direction of growth.
- The hardware can also support sharing by storing extra protection bits for each segment.
	- This allows processes to share segments in physical memory to save space.

## Operating System Support
- Extends [[Base and Bounds#Operating System Support]]
- Save/restore segment registers correctly before running a process.
- Support the growth of segments.

## Advantages
- Extends [[Base and Bounds#Advantages]]
- Supports sparse address spaces by avoiding the huge waste of memory between the logical segments of the address space.
- Supports sharing.
	- e.g, code sharing
- Supports permissions.
	- e.g, can't execute code on the stack/heap
		- Defends against [[Stack Buffer Overflow]]

## Disadvantages
- *External Fragmentation*: there might be some holes in between segments that reside in physical memory.
- Hard to allocate variable-sized segments.
- Isn't flexible enough to support a sparsely-used heap for example.
	- The entire heap must reside in memory to be accessed.

# Sources
- OSTEP Chapter 16 - "Segmentation"