# Explanation

## Motivation:
- System memories are growing.
	- As memory gets bigger, more data can be cached. Thus, write performance becomes the bottleneck.
- There is a large gap between the performance of random IO and sequential IO.
- Existing systems perform poorly on many common workloads.

## Structure
![[LFS Disk Layout.png]]

## Mechanism
- Basic idea:
	- When writing to the disk, LFS buffers all updates in an in-memory segment.
	- When the segment is full, it's written to disk in one long, sequential transfer to an unused portion of the disk.
	- LFS never overwrites existing data, but rather always writes segments to free locations.
- #inode_map : a structure that takes an #inode number as input and produces the disk address of the most recent version of the #inode.
	- Needs to be kept persistent to keep track of the disk locations of inodes across crashes.
	- LFS places a chunk of the inode map right next to where it is writing all of the other new info.
- When appending a data block `k` to a file, LFS writes the the new data block, its inode, ans a piece of the inode map all together onto the disk.
- *Checkpoint Region*: contains pointers to the latest pieces of the #inode_map.
	- Updated periodically (e.g, every 30 seconds) and thus performance is not ill-affected.
- *Recursive Update Problem*: when an inode is updated, its location on disk changes. This would have also entailed an update to the directory that points to this file, when then would have mandated a change to the parent of that directory, and so on, all the way up to the root of the file system tree.
	- Arises in any file system that never updates in place, but rather moves updates to new locations on the disk.
	- LFS avoids this problem with the help of the #inode_map.
- *Garbage Problem*: LFS leaves old version of the block of file structures scattered throughout the disk. These old versions are called **garbage**.
	- *Garbage Collection*: a technique that arises in programming languages that automatically free unused memory for programs.
	- Periodically LFS cleaner, reads in a number of old segments, determines which blocks are live within these segments, and then write out a new set of segments with just the live blocks within them, freeing up the old ones for writing.
		- How does LFS determines block liveness?
			- LFS adds a segment summary block that contains, for each segment, the inode number and the offset within the segment.

## Advantages
- Highly efficient writes.

## Disadvantages
- Old copies of files are considered garbage.

# Sources
- OSTEP Chapter 43 - "Log-structures File Systems"