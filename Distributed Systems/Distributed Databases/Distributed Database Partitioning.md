# Explanation
- Distributed database systems must partition the database across multiple resources, including disks, nodes, processors.
	- This process is sometimes called sharding in NoSQL systems.
- When the DBMS receives a query,
	1. it first analyzes the data that the query plan needs to access.
	2. The DBMS may potentially send fragments of the query plan to different nodes,
	3. then combines the results to produce a single answer.
- The goal of a partitioning scheme is to maximize single-node transactions, or transactions that only access data contained on one partition.
	- This allows the DBMS to not need to coordinate the behavior of concurrent transactions running on other nodes.
- A distributed transaction accessing data at one or more partitions will require expensive, difficult coordination.

- Types:
	- *Logical Partitioning*: a type of partitioning where particular nodes are in charge of accessing specific tuples from a shared disk.
	- *Physical Partitioning*: a type of partitioning where each shared-nothing node reads and updates tuples it contains on its own local disk.

## Naive Table Partitioning
- The simplest way to partition tables is naive data partitioning where each node stores one table, assuming enough storage space for a given node.
- This is easy to implement because a query is just routed to a specific partitioning. 
- This can be bad, since it is not scalable: one partition’s resources can be exhausted if that one table is queried on often, not using all nodes available.
- Illustration: ![[Naive Table Partitioning.png]]

## Vertical Partitioning
- *Vertical Partitioning*: a partitioning scheme which splits a table’s attributes into separate partitions.
- Each partition must also store tuple information for reconstructing the original record.

## Horizontal Partitioning
- *Horizontal Partitioning*: a partitioning scheme which splits a table’s tuples into disjoint subsets that are equal in terms of size, load, or usage, based on some partitioning key(s).

## Hash Partitioning
- The DBMS can partition a database physically (shared nothing) or logically (shared disk) via hash partitioning or range partitioning.
- The problem of hash partitioning is that when a new node is added or removed, a lot of data needs to be shuffled around.
	- The solution for this is [[Consistent Hashing]].

# Sources
- CMU 15-445 Lecture 21 - "Introduction to Distributed Databases"
