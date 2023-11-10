# Explanation
- Structures are represented as a block of contiguous memory big enough to hold all of the fields.
- Fields are ordered according to the declaration even if another ordering could yield a more compact representation.![[Structs representation in memory.png]]
- The compiler determines the overall size + position of the fields.
- To access any field of the struct in machine-level programming, the compiler generates offsets according to the relative position of the field inside the struct.
- *Alignment*
	- The CPU in modern computer hardware performs reads and writes to memory most efficiently when the data is naturally aligned, which generally means that the data's memory address is a multiple of the data size.
	- The compiler inserts gaps in structs to ensure correct alignment of fields.
	- If the biggest field in the struct is of size *K*, then the compiler aligns all the fields to be at addresses which are multiples of *K*.
	- To save space, programmers can greedily sort the struct fields from largest to smallest.

# Sources
- Extends [[Computer Architecture/Machine-level Programming/Data Structures/Data Structures#Sources]]
- [Wikipedia - Data structure alignment](https://en.wikipedia.org/wiki/Data_structure_alignment)
