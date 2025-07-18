# Explanation
- *Distributed System*: A collection of independent computers that appear to the users of the system as a single computer.

## Why do people build distributed systems?
- to increase capacity via parallelism,
- to tolerate faults via replication,
- to place computing physically close to external entities, and
- to achieve security via isolation.

## Challenges in distributed systems
- They have many concurrent parts.
- They must cope with partial failures.
- It's tricky to realize performance potential.
- Unreliable networks.
- [[Unreliable Clocks]]
- [[Byzantine Faults]]

## Topics
- [[Replication]]
	- Primary/Backup Replication
	- [[Chain Replication]]
- [[Partitioning]]
	- [[Consistent Hashing]]
- [[Consistency Guarantees]]
	- [[Linearizability]]
	- [[Eventual Consistency]]
- [[CAP Theorem]]
- [[Consensus]]
- [[Distributed Databases]]
	- [[Two-Phase Commit]]

## Papers
- [[MapReduce]]
- [[The Google File System]]
- VMware Fault-Tolerant Virtual Machines
- Raft
- Zookeeper
- Kafka
- CRAQ
- Amazon Aurora
- Frangipani

# Sources
- [MIT 6.824 - Lecture 1](https://www.youtube.com/watch?v=cQP8WApzIQQ&list=PLrw6a1wE39_tb2fErI4-WkMbsvGQk9_UB&index=1&pp=iAQB)
