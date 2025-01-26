# Explanation
- *Distributed DBMS*: a [[Database Systems#^1ec6f9|DBMS]] that divides a single logical database across multiple physical resources.
	- The application is (usually) unaware that data is split across separated hardware.
	- The system relies on the techniques and algorithms from single-node DBMSs to support transaction processing and query execution in a distributed environment.
- An important goal in designing a distributed DBMS is fault tolerance (i.e., avoiding a single one node failure taking down the entire system).

## Parallel vs Distributed Databases
- **Parallel Database**:
	- Nodes are physically close to each other.
	- Nodes are connected via high-speed LAN (fast, reliable communication fabric).
	- The communication cost between nodes is assumed to be small. As such, one does not need to worry about nodes crashing or packets getting dropped when designing internal protocols.
- **Distributed Database**:
	- Nodes can be far from each other.
	- Nodes are potentially connected via a public network, which can be slow and unreliable.
	- The communication cost and connection problems cannot be ignored (i.e., nodes can crash, and packets can get dropped).

## System Architecture
- A DBMS’s system architecture specifies what shared resources are directly accessible to CPUs.
	- It affects how CPUs coordinate with each other and where they retrieve and store objects in the database.
- ![[Database System Architectures.png]]

### Shared everything
- A single-node DBMS uses what is called a **shared everything** architecture.
- This single node executes workers on a local CPU(s) with its own local memory address space and disk.

### Shared Memory
- An alternative to shared everything architecture in distributed systems is shared memory.
	- CPUs have access to common memory address space via a fast interconnect. CPUs also share the same disk.
- In practice, most DBMSs do not use this architecture, since
	- it is provided at the [[Operating Systems|OS]]/kernel level, and
	- it also causes problems, since each process’s scope of memory is the same memory address space, which can be modified by multiple processes.
- Each processor has a global view of all the in-memory data structures.
- Each DBMS instance on a processor has to “know” about the other instances.

### Shared Disk
- In a shared disk architecture, all CPUs can read and write to a single logical disk directly via an interconnect, but each have their own private memories.
	- The local storage on each compute node can act as caches.
	- This approach is more common in cloud-based DBMSs.
- The DBMS’s execution layer can scale independently from the storage layer.
	- Adding new storage nodes or execution nodes does not affect the layout or location of data in the other layer.
- Nodes must send messages between them to learn about other node’s current state.
	- That is, since memory is local, if data is modified, changes must be communicated to other CPUs in the case that piece of data is in main memory for the other CPUs.
- Nodes have their own buffer pool and are considered stateless.
	- A node crash does not affect the state of the database since that is stored separately on the shared disk.
	- The storage layer persists the state in the case of crashes.

### Shared Nothing
- In a shared nothing environment, each node has its own CPU, memory, and disk.
	- Nodes only communicate with each other via network.
	- Before the rise of cloud storage platforms, the shared nothing architecture used to be considered the correct way to build distributed DBMSs.
- It is more difficult to increase capacity in this architecture because the DBMS has to physically move data to new nodes.
- It is also difficult to ensure consistency across all nodes in the DBMS, since the nodes must coordinate with each other on the state of transactions.
- The advantage is that shared nothing DBMSs can potentially achieve better performance and are more efficient then other types of distributed DBMS architectures.

## Partitioning Schemes
- Distributed database systems must partition the database across multiple resources, including disks, nodes, processors.
	- This process is sometimes called sharding in NoSQL systems.
- When the DBMS receives a query,
	1. it first analyzes the data that the query plan needs to access.
	2. The DBMS may potentially send fragments of the query plan to different nodes,
	3. then combines the results to produce a single answer.
- The goal of a partitioning scheme is to maximize single-node transactions, or transactions that only access data contained on one partition.
	- This allows the DBMS to not need to coordinate the behavior of concurrent transactions running on other nodes.
- A distributed transaction accessing data at one or more partitions will require expensive, difficult coordination.

- *Logical Partitioning*: a type of partitioning where particular nodes are in charge of accessing specific tuples from a shared disk.
- *Physical Partitioning*: a type of partitioning where each shared-nothing node reads and updates tuples it contains on its own local disk.

### Naive Table Partitioning
- The simplest way to partition tables is naive data partitioning where each node stores one table, assuming enough storage space for a given node.
- This is easy to implement because a query is just routed to a specific partitioning. 
- This can be bad, since it is not scalable: one partition’s resources can be exhausted if that one table is queried on often, not using all nodes available.
- Illustration: ![[Naive Table Partitioning.png]]

### Vertical Partitioning
- *Vertical Partitioning*: a partitioning scheme which splits a table’s attributes into separate partitions.
- Each partition must also store tuple information for reconstructing the original record.

### Horizontal Partitioning
- *Horizontal Partitioning*: a partitioning scheme which splits a table’s tuples into disjoint subsets that are equal in terms of size, load, or usage, based on some partitioning key(s).

### Hash Partitioning
- The DBMS can partition a database physically (shared nothing) or logically (shared disk) via hash partitioning or range partitioning.
- The problem of hash partitioning is that when a new node is added or removed, a lot of data needs to be shuffled around.
	- The solution for this is [[Consistent Hashing]].

## Distributed Concurrency Control
- A distributed transaction accesses data at one or more partitions, which requires expensive coordination.

### Centralized coordinator
- The centralized coordinator acts as a global “traffic cop” that coordinates all the behavior.
- Illustration: ![[Centralized Coordinator.png]]

### Middleware
- Centralized coordinators can be used as middleware, which accepts query requests and routes queries to correct partitions.

### Decentralized coordinator
- In a decentralized approach, nodes organize themselves.
- The client directly sends queries to one of the partitions.
- This home partition will send results back to the client.
	- The home partition is in charge of communicating with other partitions and committing accordingly.

# Sources
- CMU 15-445 Lecture 21 - "Introduction to Distributed Databases"