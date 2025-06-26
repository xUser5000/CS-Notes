# Explanation
- *Serializable Snapshot Isolation*: an #optimistic [[Concurrency Control Protocols|concurrency control protocol]] that provides full [[Concurrency Control Protocols#^5d741a|serializability]], but has only a small performance penalty compared to [[Transaction Isolation Levels#Snapshot Isolation|snapshot isolation]].
- It's based on snapshot isolation and [[Multi-versioning Concurrency Control (MVCC)|MVCC]].
- On top of snapshot isolation, SSI adds an algorithm for detecting serialization conflicts among writes and determining which transactions to abort.

## Advantages
- if there is enough spare capacity, and if contention between transactions is not too high, optimistic concurrency control techniques tend to perform better than #pessimistic ones.

## Disadvantages
- It performs badly if there is high contention (many transactions trying to access the same objects), as this leads to a high proportion of transactions needing to abort.
	- If the system is already close to its maximum throughput, the additional transaction load from retried transactions can make performance worse.

# Sources
- Designing Data-Intensive Applications - Chapter 7
