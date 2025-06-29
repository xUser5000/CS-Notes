# Explanation
- *Replicated State Machine*: primary receives operations from clients and sends them to all other replicas.
	- All replicas execute all operations.
	- If all the replicas start at the same state, execute the same operations in the same order, and they're deterministic, then all replicas will arrive at the same end state.
- Often generates less network traffic because operations are often small compared to state but it's complex to get right.

# Sources
- [MIT 6.824 - Lecture 4](https://www.youtube.com/watch?v=M_teob23ZzY)
