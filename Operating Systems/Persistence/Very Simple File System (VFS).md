# Explanation

## Structure
- Divides the disk into fixed-size blocks (commonly 4 KB).
- Divides the disk into regions, each serving a different purpose.
	- Illustration: ![[VFS on-disk Data Structures.png]]
		- *Data Region*: used for storing file data.
		- *Inode Table*: holds an array of inodes.
			- #inode: used for storing metadata such as file size, owner, access rights, access and modify timestamps, etc... 
		- *Bitmap*: each bit indicates whether the corresponding block is free or used.
		- *Super Block*: contains information about this particular file system, e.g,
			- how many inodes and data blocks there are
			- where the inode table begins
			- magic number to identify the file system type.
- There are several approaches for the #inode to point to data blocks:
	- *Single-level Index*
		- **Description**: make the inode point to a fixed number of data blocks.
		- **Advantages**: Simple.
		- **Disadvantages**: Can't store a file whose size > `no. pointers x sizeof(block)`
	- *Multi-level Index*
		- **Description**:
			- Have a special indirect pointer that points to a block that contains more pointers.
			- inode may have a fixed number of direct pointer and a single indirect pointer.
				- If a file grows large enough, an indirect block is allocated and the inode's slot for an indirect pointer is set to point to it.
			- Used by Linux ext2 and ext3.
		- **Advantages**:
			- accommodates large files.
			- Uses an imbalanced tree due to the fact that most files are small.
		- **Disadvantages**: 
- Directories are stored as a special type of file whose data block contains a list of (entry name, inode number) pairs.

## Mechanism
- When mounting a file system (refer to [[File Systems#Interface]]), the OS will read the super block first, to initialize various parameters and attach the volume to the file system tree.
- File systems manages free space using the bitmaps stored on disk.
- File systems aggressively uses RAM to cache important blocks.
	- Modern operating systems integrate virtual memory pages and file system pages into a unified page cache.
- **Write buffering**: File systems can delay writes to the disk. Thus, file systems can make use of some optimization tricks like:
	- batching updates into a smaller set of IOs
	- scheduling requests 
	- avoiding some writes altogether
		- e.g, when writing to a file and then deleting it

## Advantages
- Simple
- Working

## Disadvantages
- Terrible performance
- Not disk-aware
- Fragmentation
	- Happens when freeing up space or when files are deleted
	- Disk defragmentation tools can help solve this by reorganizing on-disk data to place files contiguously and make free space.

# Sources
- OSTEP Chapter 40 - "File System Implementation"