# Explanation
- A DBMS’s system architecture specifies what shared resources are directly accessible to CPUs.
	- It affects how CPUs coordinate with each other and where they retrieve and store objects in the database.
- Types: ![[Database System Architectures.png]]

## Shared everything
- A single-node DBMS uses what is called a **shared everything** architecture.
- This single node executes workers on a local CPU(s) with its own local memory address space and disk.

## Shared Memory
- An alternative to shared everything architecture in distributed systems is shared memory.
	- CPUs have access to common memory address space via a fast interconnect. CPUs also share the same disk.
- In practice, most DBMSs do not use this architecture, since
	- it is provided at the [[Operating Systems|OS]]/kernel level, and
	- it also causes problems, since each process’s scope of memory is the same memory address space, which can be modified by multiple processes.
- Each processor has a global view of all the in-memory data structures.
- Each DBMS instance on a processor has to “know” about the other instances.

## Shared Disk
- In a shared [[Disks|disk]] architecture, all CPUs can read and write to a single logical disk directly via an interconnect, but each have their own private memories.
	- The local storage on each compute node can act as caches.
	- This approach is more common in cloud-based DBMSs.
- The DBMS’s execution layer can scale independently from the storage layer.
	- Adding new storage nodes or execution nodes does not affect the layout or location of data in the other layer.
- Nodes must send messages between them to learn about other node’s current state.
	- That is, since memory is local, if data is modified, changes must be communicated to other CPUs in the case that piece of data is in main memory for the other CPUs.
- Nodes have their own buffer pool and are considered stateless.
	- A node crash does not affect the state of the database since that is stored separately on the shared disk.
	- The storage layer persists the state in the case of crashes.

## Shared Nothing
- In a shared nothing environment, each node has its own CPU, memory, and disk.
	- Nodes only communicate with each other via network.
	- Before the rise of cloud storage platforms, the shared nothing architecture used to be considered the correct way to build distributed DBMSs.
- It is more difficult to increase capacity in this architecture because the DBMS has to physically move data to new nodes.
- It is also difficult to ensure consistency across all nodes in the DBMS, since the nodes must coordinate with each other on the state of transactions.
- The advantage is that shared nothing DBMSs can potentially achieve better performance and are more efficient then other types of distributed DBMS architectures.

# Sources
- CMU 15-445 Lecture 21 - "Introduction to Distributed Databases"