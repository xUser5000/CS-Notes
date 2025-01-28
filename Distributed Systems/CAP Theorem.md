# Explanation

> *The CAP Theorem*: it is impossible for a distributed system to always be **Consistent**, **Available**, and **Partition-Tolerant**. Only two of these three properties can be chosen.

---

### Consistency
- [[Linearizability#^7f4541|Consistency]] is synonymous with [[Linearizability|linearizability]] for operations on all nodes.
- Once a write completes, all future reads should return the value of that write applied or a later write applied.
- Additionally, once a read has been returned, future reads should return that value or the value of a later applied write.
- NoSQL systems compromise this property in favor of the latter two.
	- Other systems will favor this property and one of the latter two.

### Availability
- *Availability*: the concept that all nodes that are up can satisfy all requests.

### Partition-Tolerance
- *Partition tolerance*: the system can still operate correctly despite some message loss between nodes that are trying to reach consensus on values.

## Insights
- If consistency and partition-tolerance is chosen for a system, updates will not be allowed until a majority of nodes are reconnected, typically done in traditional or NewSQL DBMSs.
- There is a modern version that considers consistency vs. latency trade-offs: *PACELC Theorem*.
	- In case of **network partitioning** (P) in a distributed system, one has to choose between **availability** (A) and **consistency** (C), else (E), even when the system runs normally without network partitions, one has to choose between **latency** (L) and **consistency** (C).

# Sources
- CMU 15-445 Lecture 22 - "Distributed Transactional Database Systems"