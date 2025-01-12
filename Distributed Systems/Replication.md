# Explanation
- *Replication*: creating duplicate copies of data or services across multiple nodes.
- Replication can tolerate **fail-stop** failures of a single replica. Examples:
	- Fan stops working, CPU overheats and shuts itself down,
	- Someone trips over replica's power cord or network cable, or
	- Software notices it is out of disk space and stops.
- It can't tolerate defects in hardware or bugs in software or human configuration errors because they're often not fail-stop and may be correlated (i.e. cause all replicas to crash at the same time).
- It can tolerate catastrophes (earthquake or city-wide power failure) only if the replicas are physically separated.

## Approaches

### State Transfer
- *State Transfer*: primary replica executes the service and sends *new* state to backups.
- It's simpler but the state may be large, which means it's slow to transfer over the network.

### Replicated State Machine
- *Replicated State Machine*: primary receives operations from clients and sends them to all other replicas.
	- All replicas execute all operations.
	- If all the replicas start at the same state, execute the same operations in the same order, and they're deterministic, then all replicas will arrive at the same end state.
- Often generates less network traffic because operations are often small compared to state but it's complex to get right.

## Replication Levels
- **Application State**: e.g., database tables.
	- Can be efficient; primary only sends high-level operations to backup.
	- The downside is application code (server) must understand fault tolerance, to e.g. forward operations stream.
	- [[Google File System|GFS]] works this way.
- **Machine level**: e.g. registers and RAM content
	- Might allow us to replicate any existing server without modifications.
	- Downsides:
		- requires forwarding of machine events (interrupts, DMA, &c), and
		- requires "machine" modifications to send/receive event stream.
		- [[Fault-tolerant Virtual Machines|VM-FT]] works this way.

# Sources
- [MIT 6.824 - Lecture 4](https://www.youtube.com/watch?v=M_teob23ZzY)
