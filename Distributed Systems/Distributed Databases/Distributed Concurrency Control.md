# Explanation
- A distributed transaction accesses data at one or more partitions, which requires expensive coordination.

## Coordination Approaches
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

## Distributed Transactions
- *Distributed [[Concurrency Control#^dae2be|Transaction]]*: a transactions that accesses data on multiple nodes. 
- Executing distributed transactions is more challenging than single-node transactions because now when the transaction commits, the DBMS has to make sure that all the nodes agree to commit the transaction.
- The DBMS ensures that the database provides the same [[Concurrency Control#ACID|ACID]] guarantees as a single-node DBMS even in the case of node failures, message delays, or message loss.
- One can assume that all nodes in a distributed DBMS are well-behaved and under the same administrative domain.
	- In other words, given that there is not a node failure, a node which is told to commit a transaction will commit the transaction.
	- If the other nodes in a distributed DBMS cannot be trusted, then the DBMS needs to use a byzantine fault tolerant protocol (e.g., blockchain) for transactions.

## Atomic Commit Protocols
- When a multi-node transaction finishes, the DBMS needs to ask all of the nodes involved whether it is safe to commit.
- Depending on the protocol, a majority of the nodes or all of the nodes may be needed to commit.
- Examples:
	- [[Two-Phase Commit]] (Common)
	- Three-Phase Commit (Uncommon)
	- Paxos (Common)
	- Raft (Common)
	- ZAB (Zookeeper Atomic Broadcast protocol, Apache Zookeeper)
	- Viewstamped Replication (first provably correct protocol)

# Sources
- CMU 15-445 Lecture 21 - "Introduction to Distributed Databases"
- CMU 15-445 Lecture 22 - "Distributed Transactional Database Systems"