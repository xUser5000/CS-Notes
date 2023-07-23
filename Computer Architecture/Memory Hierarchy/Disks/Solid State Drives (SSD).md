# Explanation
- A solid-state drive (SSD) is a storage device that uses integrated circuit assemblies to store data persistently, typically using flash memory (refer to [[Volatile Memory]]), and functioning as secondary storage in the [[Memory Hierarchy]].
- Structure: ![[What's inside an SSD.png]]
	- Data is read/written in units of pages.
	- Pages: 512KB to	4KB, Blocks: 32 to 128 pages.
	- A page can be written only after its block has been erased.
	- A block wears out after about 100,000 repeated writes.
- Performance characteristics: ![[SSD performance characteristics.png]]
	- A common theme is that sequential reads/writes is faster than random reads/writes.
- Advantages:
	- No moving parts, which means performance does not degrade over time
	- Faster and consume less power than [[Hard Disk Drives (HDD)]]
- Disadvantages:
	- Have the the potential to wear out
	- Much more expensive

# Sources
- Extends [[Disks#Sources]]