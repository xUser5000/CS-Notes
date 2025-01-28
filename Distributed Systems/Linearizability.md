# Explanation
- *Linearizability*: an execution history is linearizable if one can find a total order of all operations,
	- that matches real-time (for non-overlapping ops), and
	- in which each read sees the value from the write preceding it in the order.
- *Execution History*: a record of client operations, each with arguments, return value, time of start, time completed.
- *Strong Consistency*: when a distributed system behaves the same way that a system with only one node would behave. ^7f4541
	- The term is often used interchangeably with Linearizability.

## Notes
- The definition is based on external behavior so we can apply it without having to know how service works.
- Systems can choose any order for concurrent writes but all clients must see the writes in the same order.
	- This is important when we have replicas or caches because they have to all agree on the order in which operations occur.
- According to the definition, reads must return fresh data: stale values aren't linearizable.
- Linearizability forbids many situations:
	- split brain (two active leaders),
	- forgetting committed writes after a reboot, and
	- reading from lagging replicas.

# Sources
- [MIT 6.824 - Lecture 7](https://www.youtube.com/watch?v=4r8Mz3MMivY)
