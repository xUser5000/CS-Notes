# Explanation
- File systems presents a a virtual view of the disk, transforming from a bunch of raw blocks into much more user-friendly files and directories.
	- File System Stack: ![[File System Stack.png]]
- #file: linear array of bytes, each of which can be read or written to.
- Each file has a low-level name called the #inode number (short for index node).
	- Not available to users.
- #directory: a special type of file that contains a list of (user-readable name, #inode number) pairs.
	- Directories, like files, has #inode numbers.
- #file_descriptor: a pointer to an object of type #file.
	- Used to make system calls such as `read()`, `write()`, etc...
	- Managed by the OS on a per-process basis.
- Each process maintains an array of file descriptors, each of which refer to an entry in the #open_file_table.
- #open_file_table:
	- **Description**: A system-wide table that tracks:
		- which underlying file each #file_descriptor refers to.
		- current offset:
			- indicates where the next `read()` will start.
			- can be explicitly set by the process by calling `lseek()`
		- whether the file is readable or writable
		- reference count of each file
	- **Illustration**: ![[Open File Table.png]]
- Each running process already has three open files: `stdin`, `stdout`, and `stderr`.
	- Have file descriptors 0, 1, and 2, respectively.
- When a parent process creates a child process with `fork()`, all #open_file_table  entries owned by the parent pcoess get inherited down to the child process.
	- When an entry is shared, its reference count is incremented.
	- When the reference count = 0, the OS removes the file entry.

## Interface
- `read()`: reads the next chunk of the data and increments the file offset.
- `fsync()`: a system calls that forces all dirty data to disk.
	- Commonly used in Database Management Systems.
- `unlink()`: removes a file
- `link()`: creates a hard links between an old link and a new one.
	- creates another way to refer to the same file.
		- i.e, creates another human-readable path that points to the same #inode.
	- does not copy the file.
- `mount()`: takes an existing directory and as a target mount point and pastes a new file system onto the directory tree at that point.

## Implementations
- Two aspects of file system implementation:
	- **Data Structures**: what on-disk structures are utilized by the file system to organize its data and metadata.
	- **Access Methods**: how does the file system map the IO system calls made by processes onto its data structures.
- Possible implementations:
	- [[Very Simple File System (VFS)]]
	- [[Fast File System (FFS)]]
	- [[Log-structured File System (LFS)]]

## Issues
- [[Crash Consistency]]

# Sources
- OSTEP Chapter 39 - "Interlude: Files and Directories"