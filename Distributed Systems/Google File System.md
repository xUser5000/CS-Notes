# Explanation

## Abstract
- The Google File System, a scalable distributed file system for large distributed data-intensive applications.
- It provides fault tolerance while running on inexpensive commodity hardware, and it delivers high aggregate performance to a large number of clients.

## Introduction
- GFS shares many of the same goals as previous distributed file systems such as performance, scalability, reliability, and availability.
- However, its design has been driven by key observations of Google's application workloads and technological environment, both current and anticipated, that reflect a marked departure from some earlier file system design assumptions.

### Assumptions
- **Component failures are the norm rather than the exception**
	- Problems can be caused by application bugs, operating system bugs, human errors, and the failures of disks, memory, connectors, networking, and power supplies. 
	- Constant monitoring, error detection, fault tolerance, and automatic recovery must be integral to the system.
- **Files are huge by traditional standards. Multi-GB files are common**
	- As a result, design assumptions and parameters such as I/O operation and block sizes have to be revisited.
- **Most files are mutated by appending new data rather than overwriting existing data**
	- Random writes within a file are practically non-existent.
	- Once written, the files are only read, and often only sequentially.
- **Co-designing the applications and the file system API benefits the overall system by increasing our flexibility**
	- For example, GFS has a relaxed consistency model to vastly simplify the file system without imposing an onerous burden on the applications.
	- Also, atomic append operation was introduced so that multiple clients can append concurrently to a file without extra synchronization between them.

## Design Overview

### Goals
- **The system is built from many inexpensive commodity components that often fail.**
	- It must constantly monitor itself and detect, tolerate, and recover promptly from component failures on a routine basis.
- **The system stores a modest number of large files.**
	- We expect a few million files, each typically 100 MB or larger in size. Multi-GB files are the common case and should be managed efficiently.
	- Small files must be supported, but we need not optimize for them.
- **The workloads primarily consist of two kinds of reads: large streaming reads and small random reads**.
- **The workloads also have many large, sequential writes that append data to files**.
	- Once written, files are seldom modified again. Small writes at arbitrary positions in a file are supported but do not have to be efficient.
- **The system must efficiently implement well-defined semantics for multiple clients that concurrently append to the same file**.
	- Our files are often used as producer-consumer queues or for many-way merging.
	- Hundreds of producers, running one per machine, will concurrently append to a file. Atomicity with minimal synchronization overhead is essential.
	- The file may be read later, or a consumer may be reading through the file simultaneously.
- **High sustained bandwidth is more important than low latency**.
	- Most of our target applications place a premium on processing data in bulk at a high rate, while few have stringent response time requirements for an individual read or write.

### Interface
- GFS provides a familiar file system interface, though it does not implement a standard API such as POSIX.
- Files are organized hierarchically in directories and identified by pathnames.
- We support the usual operations to create, delete, open, close, read, and write files.
- Moreover, has two additional operations:
	- **Snapshot**: creates a copy of a file or a directory tree at low cost.
	- **Record append**: allows multiple clients to append data to the same file concurrently while guaranteeing the atomicity of each individual client’s append.

### Architecture
- Illustration: ![[GFS Architecture.png]]
- A GFS cluster consists of a single master and multiple chunkservers and is accessed by multiple clients.
	- Each of these is typically a commodity Linux machine running a user-level server process.
- Files are divided into fixed-size chunks.
	- Each chunk is identified by an immutable and globally unique 64 bit chunk handle assigned by the master at the time of chunk creation.
- Chunkservers store chunks on local disks as Linux files and read or write chunk data specified by a chunk handle and byte range.
	- For reliability, each chunk is replicated on multiple chunkservers.
	- By default, we store three replicas, though users can designate different replication levels for different regions of the file namespace.
- The master maintains all file system metadata including: the namespace, access control information, the mapping from files to chunks, and the current locations of chunks.
	- It also controls system-wide activities such as chunk lease management, garbage collection of orphaned chunks, and chunk migration between chunkservers.
- The master periodically communicates with each chunkserver in HeartBeat messages to give it instructions and collect its state.
- GFS client code linked into each application implements the file system API and communicates with the master and chunkservers to read or write data on behalf of the application.
	- Clients interact with the master for metadata operations, but all data-bearing communication goes directly to the chunkservers.
- Neither the client nor the chunkserver caches file data.
	- Client caches offer little benefit because most applications stream through huge files or have working sets too large to be cached.
	- Not having client caches simplifies the client and the overall system by eliminating cache coherence issues.
		- Clients do cache metadata, however.
	- Chunkservers need not cache file data because chunks are stored as local files and so Linux’s buffer cache already keeps frequently accessed data in memory.

### Single Master
- Having a single master vastly simplifies our design and enables the master to make sophisticated chunk placement and replication decisions using global knowledge.
- Clients never read and write file data through the master. Instead, a client asks the master which chunkservers it should contact.
	- It caches this information for a limited time and interacts with the chunkservers directly for many subsequent operations.

### Metadata
- The master stores three major types of metadata: the file and chunk namespaces, the mapping from files to chunks, and the locations of each chunk’s replicas.
- All metadata is kept in the master’s memory.
- Namespaces and file-to-chunk mapping are also kept persistent by logging mutations to an operation log stored on the master’s local disk and replicated on remote machines.
	- Using a log allows us to update the master state simply, reliably, and without risking inconsistencies in the event of a master crash.
- The master does not store chunk location information persistently. Instead, it asks each chunkserver about its chunks at master startup and whenever a chunkserver joins the cluster.

#### In-Memory Data Structures
- Since metadata is stored in memory, master operations are fast.
	- Therefore, It is easy and efficient for the master to periodically scan through its entire state in the background.
	- This periodic scanning is used to implement chunk garbage collection, re-replication in the presence of chunkserver failures, and chunk migration to balance load and disk space usage across chunkservers.
- One potential concern for this memory-only approach is that the number of chunks and hence the capacity of the whole system is limited by how much memory the master has.
	- This is not a serious limitation in practice because the master maintains less than 64 bytes of metadata for each 64 MB chunk.

#### Chunk Locations
- The master does not keep a persistent record of which chunkservers have a replica of a given chunk.
	- It simply polls chunkservers for that information at startup.
	- The master can keep itself up-to-date thereafter because it controls all chunk placement and monitors chunkserver status with regular HeartBeat messages.
- This eliminates the problem of keeping the master and chunkservers in sync as chunkservers join and leave the cluster, change names, fail, restart, and so on.

#### Operation Log
- The operation log contains a historical record of critical metadata changes.
- It is central to GFS. Not only is it the only persistent record of metadata, but it also serves as a logical time line that defines the order of concurrent operations. 
- Files and chunks, as well as their versions are all uniquely and eternally identified by the logical times at which they were created.

## System Interactions
// TODO

# Sources
- [The Google File System](http://nil.csail.mit.edu/6.824/2020/papers/gfs.pdf)
- [MIT 6.824 - Lecture 3](https://www.youtube.com/watch?v=EpIgvowZr00&list=PLrw6a1wE39_tb2fErI4-WkMbsvGQk9_UB&index=3)
