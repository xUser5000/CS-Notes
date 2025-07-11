# Explanation [1]
- *Linearizability*: an execution history is linearizable if one can find a total order of all operations,
	- that matches real-time (for non-overlapping ops), and
	- in which each read sees the value from the write preceding it in the order.
- *Execution History*: a record of client operations, each with arguments, return value, time of start, time completed.
- *Strong Consistency*: when a distributed system behaves the same way that a system with only one node would behave. ^7f4541
	- The term is often used interchangeably with Linearizability.

## Notes
- The definition is based on external behavior so we can apply it without having to know how service works. [1]
- Systems can choose any order for concurrent writes but all clients must see the writes in the same order. [1]
	- This is important when we have replicas or caches because they have to all agree on the order in which operations occur.
- According to the definition, reads must return fresh data: stale values aren't linearizable. [1]
- Linearizability forbids many situations: [1]
	- split brain (two active leaders),
	- forgetting committed writes after a reboot, and
	- reading from lagging replicas.

- Consensus algorithms are linearizable. [2]
- Single-leader replication is potentially linearizable. [2]
- Multi-leader replication is not linearizable. [2]

## Linearizability Vs Serializability [2]
- **Linearizability**: recency guarantee on reads and writes of a register (an individual object).
	- It doesn’t group operations together into transactions, so it does not prevent problems such as [[Transaction Anomalies#Write Skew [1]|write skew]].
- **[[Transaction Isolation Levels#Serializable|Serializability]]**: isolation property of [[Concurrency Control#^68a44d|transactions]], where every transaction may read and write multiple objects.
	- It guarantees that transactions behave the same as if they had executed in some serial order.
	- It is okay for that serial order to be different from the order in which transactions were actually run.

- A database may provide both serializability and linearizability, and this combination is known as *strict serializability* or *strong one-copy serializability*.
- Implementations of serializability based on [[Two-Phase Locking|two-phase locking]] or actual serial execution are typically linearizable.
- However, [[Serializable Snapshot Isolation (SSI)|serializable snapshot isolation]] is not linearizable: by design, it makes reads from a consistent snapshot, to avoid lock contention between readers and writers.
	- The whole point of a consistent snapshot is that it does not include writes that are more recent than the snapshot, and thus reads from the snapshot are not linearizable.

# Sources
1. [MIT 6.824 - Lecture 7](https://www.youtube.com/watch?v=4r8Mz3MMivY)
2. Designing Data-Intensive Applications - Chapter 9
