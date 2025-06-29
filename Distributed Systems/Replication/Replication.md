# Explanation
- *Replication*: creating duplicate copies of data or services across multiple nodes. [1]
- Each node that stores a copy of the database is called a *replica*. [2] ^b57081
- Replication can tolerate **fail-stop** failures of a single replica. Examples: [1]
	- Fan stops working, CPU overheats and shuts itself down,
	- Someone trips over replica's power cord or network cable, or
	- Software notices it is out of disk space and stops.
- It can't tolerate defects in hardware or bugs in software or human configuration errors because they're often not fail-stop and may be correlated (i.e. cause all replicas to crash at the same time). [1]
- It can tolerate catastrophes (earthquake or city-wide power failure) only if the replicas are physically separated. [1]

## Use Cases [2]
- To keep data geographically close to your users (and thus reduce access latency),
- To allow the system to continue working even if some of its parts have failed (and thus increase availability), and
- To scale out the number of machines that can serve read queries (and thus increase read throughput).

## Topics
- Algorithms
	- [[Single-leader Replication]]
	- [[Multi-leader Replication]]
	- [[Leaderless Replication]]
- [[Synchronous vs Asynchronous Replication]]
- [[Handling Node Outages in Replication]]
- [[Implementation of Replication Logs]]
- [[Replication Quorums]]
- [[Replicated State Machine]]
- [[Replication Levels]]

# Sources
1. [MIT 6.824 - Lecture 4](https://www.youtube.com/watch?v=M_teob23ZzY)
2. Designing Data-Intensive Applications - Chapter 5
