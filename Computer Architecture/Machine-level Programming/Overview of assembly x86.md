# Explanation
- *Instruction Set Architecture*: The parts of a processor design that one needs to understand to write assembly/machine code.
		- e.g, Intel x86, IA32, x86-64, ARM
	- *Micro-architecture*: The hardware implementation of the instruction set architecture.
	- *Machine Code*: The byte-level programs that are executed by a processor.
	- *Assembly Code*: A textual representation of machine code.
- State visible to the machine-level programmer:![[State visible to the machine-level programmer.png]]
- Data Types:
	- Integer data of 1, 2, 4, or 8 bytes that might represent data values or memory addresses.
	- Floating-point data of 4, 8, or 10 bytes.
	- Code: byte sequences encoding a series of instructions.
	- **Note**: There are no aggregate data types (e.g arrays or structs) in assembly x86. They are represented a contiguously allocated bytes in memory.
- An instruction in x86 can do the following:
	- Perform an arithmetic operation on a register or memory data.
	- Transfer data between back and forth between memory and registers (e.g `movq`).
	- Transfer control (e.g conditional and unconditional jumps)

# Sources
- [CMU intro to Computer Systems: Lecture 05](https://scs.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=6e410255-3858-4e85-89c7-812c5845d197)