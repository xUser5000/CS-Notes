# Explanation
- A typical computer system consists of different levels of storage devices:
	- CPU registers
	- [[CPU Cache]]
	- Main memory
	- Local secondary storage (e.g, [[Disks]] and [[RAID]])
		- [[Hard Disk Drives (HDD)]]
		- [[Solid State Drives (SSD)]]
	- Remote secondary storage
- Illustration: ![[Memory Hierarchy.png]]
- These levels are backed by different types of Storage Technologies:
	- [[Volatile Memory]] 
	- [[Non-volatile Memory]]
- **The big idea**: For each k, the faster, smaller device at level k serves as a [[Cache]] for the larger, slower device at level k+1.
- The memory hierarchy creates a large pool of storage that costs as much as the cheap storage near the bottom, but that serves data to programs at the rate of the fast storage at the top.
- Average access times of different levels in the hierarchy: ![[Access times in memory hierarchy.png]]

# FAQ
- Why do memory hierarchies work?
	- Because of [[Locality of reference]], programs tend to access the data at level k more often than they access data at level k+1.

# Sources
-  [CMU Introduction to Computer Systems - Lecture 11](https://scs.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=06dfcd19-1024-49eb-add8-3486a38d1426)