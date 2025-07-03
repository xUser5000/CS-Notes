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

# Topics
- [[Distributed Database Architecture]]
- [[Distributed Database Partitioning]]
- [[Distributed Concurrency Control]]

# Sources
- CMU 15-445 Lecture 21 - "Introduction to Distributed Databases"