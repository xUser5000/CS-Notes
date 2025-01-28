# Explanation

## Mechanism

### Phase 1: Prepare
1. The client sends a Commit Request to the [[Distributed Concurrency Control#Coordination Approaches|coordinator]].
2. In the first phase of this protocol, the coordinator sends a Prepare message, essentially asking the participant nodes if the current transaction is allowed to commit.
3. If a given participant verifies that the given transaction is valid, they send an OK to the coordinator.
4. If the coordinator receives an OK from all the participants, the system can now go into the second phase in the protocol.
5. If anyone sends an Abort to the coordinator, the coordinator sends an Abort to the client.

### Phase 2: Commit
1. The coordinator sends a Commit to all the participants, telling those nodes to commit the transaction, if all the participants sent an OK.
2. Once the participants respond with an OK, the coordinator can tell the client that the transaction is committed.
3. If the transaction was aborted in the first phase, the participants receive an Abort from the coordinator, to which they should respond to with an OK.

- **Notes**:
	- Either everyone commits or no one does.
	- The coordinator can also be a participant in the system.

### Crash Recovery
- In the case of a crash, all nodes keep track of a non-volatile log of the outcome of each phase.
- Nodes block until they can figure out the next course of action.
- If the coordinator crashes, the participants must decide what to do.
	- A safe option is just to abort.
	- Alternatively, the nodes can communicate with each other to see if they can commit without the explicit permission of the coordinator.
- If a participant crashes, the coordinator assumes that it responded with an abort if it has not sent an acknowledgement yet.

## Optimizations
- **Early Prepare Voting**: If the DBMS sends a query to a remote node that it knows will be the last one executed there, then that node will also return their vote for the prepare phase with the query result.
- **Early Acknowledgement after Prepare**: If all nodes vote to commit a transaction, the coordinator can send the client an acknowledgement that their transaction was successful before the commit phase finishes.

# Sources
- CMU 15-445 Lecture 22 - "Distributed Transactional Database Systems"
