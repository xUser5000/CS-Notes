# Explanation
- Disks are [[IO Devices]] that are usually used as the secondary storage device in a computer system.
- Types of disks:
	- [[Hard Disk Drives (HDD)]]
	- [[Solid State Drives (SSD)]]
- Modern disks present a simpler abstract view of the complex sector geometry.
	- The set of available sectors are modeled as a sequence of b-sized logical blocks.
	- The disk controller (firmware) converts block requests into physical (surface, track, sector) triple.

# Sources
- Extends [[Memory Hierarchy#Sources]]