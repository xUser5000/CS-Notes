# Explanation
- The goal is to make the file system "disk-aware".
- Used in ext2 and ext3.

## Structure
- FFS divides the disk into multiple block groups.
- A block group is a consecutive portion in the disks's address space.
	- Central mechanism used by FFS to improve performance.
	- By placing two files within the same group, FFS ensure that accessing one after the other will not result in long seeks across the disk.
- Each group contains all the structures of the [[Very Simple File System (VFS)]].
	- keeps a copy of the super block within each group for reliability reasons.

## Mechanism
- The basic idea is simple: "Keep related stuff together".
- To allocate a new directory, FFS picks the group with the lowest number of allocated directories, then it puts the directory data and the inode in that group.
- FFS allocates the inode and the data of the file in the same group.
- FFS places all files that are in the dame directory in the group of the directory they are in.
- **Issue**: a large file might fill the block group entirely with its data, which is undesirable.
	- To solve this, FFS puts additional chunks in other block groups.

## Advantages
- Extends [[Very Simple File System (VFS)#Advantages]]
- improved performance due to its disk-awareness
- longer file names
- symbolic links
- atomic file renaming

## Disadvantages


# Sources
- OSTEP Chapter 41 - "Locality and The Fast File System"