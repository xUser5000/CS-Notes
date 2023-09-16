# Explanation
- A TLB is a hardware [[Cache]] of popular virtual to physical address translations.
	- part of the MMU.
	- stands for "translation look-aside buffer", but a better name would be "address translation cache".
- TLBs make virtual memory possible because of their tremendous performance impact.
- Like any [[Cache]], TLBs rely upon both spatial and temporal [[Locality of reference]].
 
## Mechanism
- Upon each virtual memory reference, the hardware first checks the TLB to see if the desired translation is held therein; if so, the translation is performed (quickly) without having to consult the page table (which has all translations).
	- Illustration: ![[TLB Control Flow Algorithm.png]]
- There are two ways to handle a TLB miss:
	- *Hardware-managed TLB*:
		- The hardware has to know exactly where the page tables are located in memory via a *page table base register* as well as their exact format.
		- On a miss, the hardware would
			1. “walk” the page table
			2. find the correct page-table entry
			3. extract the desired translation
			4. update the TLB with the translation
			5. retry the instruction
	- *Software-managed TLB*:
		- The hardware simply raises an exception which stops execution of the current process, and jumps into a #trap_handler. Then, the OS handles the rest.
		- Advantages:
			- More flexible
				- the OS can use any data structure it wants to implement the page table without necessitating hardware change.
			- requires little support from the hardware.

## Contents:
- VPN
- PFN
- valid bit: indicates whether the entry has a valid translation or not.
- protection bits
- address space identifier (ASID): same as the process identifier (PID) but has fewer bits.
- dirty bit
- etc...

## Issues
1. On a #context_switch, TLB contents for the last process are not meaningful for the about-to-be-run process. There are two solutions to such an issue:
	- Flush the contents of the TLB every time a #context_switch happens.
		- can be dome by sitting all valid bits to 0.
		- Disadvantage: each time a process runs, it incur TLB misses as it touches its data and code pages.
	- Share the TLB contents across #context_switch by making use of the address space identifier (ASID).
		- The hardware knows which process is currently running on the CPU by having an ASID bit on the chip that can be set by a privileged instruction.
2. Replacement Policy: some algorithms can be used such as:
	- Least Recently Used (LRU)
	- Random

# FAQ
- Extends [[CPU Cache#FAQ]]
# Sources
- OSTEP Chapter 19 - "Paging: Faster Translations (TLBs)"