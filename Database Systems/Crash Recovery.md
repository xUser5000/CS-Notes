# Explanation
- The DBMS needs to ensure the following guarantees:
	- The changes for any transaction are durable once the DBMS has told somebody that it committed.
	- No partial changes are durable if the transaction aborted.
- *Recovery algorithms*: techniques to ensure database consistency, transaction atomicity, and durability despite failures. 
- Every recovery algorithm has two parts:
	1. Actions during normal transaction processing to ensure that the DBMS can recover from a failure.
	2. Actions after a failure to recover the database to a state that ensures atomicity, consistency, and durability.
- Recovery algorithms use the following primitives (not all algorithms use both):
	- UNDO: The process of removing the effects of an incomplete or aborted transaction.
	- REDO: The process of re-applying the effects of a committed transaction for durability.

## Failures
- **Transaction Failures**: occur when a transaction reaches an error and must be aborted.
	- **Logical Errors**: A transaction cannot complete due to some internal error condition (e.g., integrity, constraint violation).
	- **Internal State Errors**: The DBMS must terminate an active transaction due to an error condition (e.g., deadlock).
- **System Failures**: unintended failures in the underlying software or hardware that hosts the DBMS.
	- **Software Failures**: There is a problem with the DBMS implementation (e.g., uncaught divide-by-zero exception) and the system has to halt.
	- **Hardware Failure**: The computer hosting the DBMS crashes (e.g., power plug gets pulled).
- **Storage Media Failure**: non-repairable failures that occur when the physical storage device is damaged.
	- When the storage media fails, the DBMS must be restored from an archived version.
	- The DBMS cannot recover from a storage failure and requires human intervention.
	- A head crash or similar disk failure destroys all or parts of non-volatile storage.

## Buffer Pool Management Policies
- *Steal Policy*: dictates whether the DBMS allows an uncommitted transaction to overwrite the most recent committed value of an object in non-volatile storage.
	- STEAL: Is allowed.
	- NO-STEAL: Is not allowed.
- *Force Policy*: dictates whether the DBMS requires that all updates made by a transaction are reflected on non-volatile storage before the transaction is allowed to commit.
	- FORCE: Is required.
	- NO-FORCE: Is not required.
- Force writes make it easier to recover since all of the changes are preserved but result in poor runtime performance.
- The easiest buffer pool management policy to implement is called NO-STEAL + FORCE.
	- The DBMS never has to undo changes of an aborted transaction because the changes were not written to disk.
	- The DBMS never has to redo changes of a committed transaction because all the changes are guaranteed to be written to disk at commit time.
	- **Limitation**: data that a transaction needs to modify must fit into memory.
		- Otherwise, that transaction cannot execute because the DBMS is not allowed to write out dirty pages to disk before the transaction commits.

## Shadow Paging
- *Shadow Paging*: the DBMS copies pages on write to maintain two separate versions of the database:
	- **master**: Contains only changes from committed txns.
	- **shadow**: Temporary database with changes made from uncommitted transactions.
- Updates are only made in the shadow copy.
- When a transaction commits, the shadow copy is atomically switched to become the new master.
	- The old master is eventually garbage collected.
- **Recovery**:
	- **Undo**: Remove the shadow pages. Leave the master and DB root pointer alone.
	- **Redo**: Not needed at all.
- **Advantages**:
	- Has faster recovery time than [[#Write-Ahead Logging]].
- **Disadvantages**:
	- Copying the entire page table is expensive.
	- Commit overhead is high.
	- Only supports one writer transaction at a time or transactions in a batch.

## Write-Ahead Logging
- *Write-Ahead Logging*: the DBMS records all the changes made to the database in a log file (on stable storage) before the change is made to a disk page.
- The log contains sufficient information to perform the necessary undo and redo actions to restore the database after a crash.
- The DBMS must write to disk the log file records that correspond to changes made to a database object before it can flush that object to disk.
- **Advantages**:
	- Allows the DBMS to do sequential writes to optimize performance.
		- Almost every DBMS uses write-ahead logging (WAL) because it has the fastest runtime performance.
- **Disadvantages**:
	- Has slower recovery time than [[#Shadow Paging]].

## Logging Schemes
// TODO

## Checkpoints
// TODO

# Sources
- CMU 15-445 Lecture 19 - "Logging Schemes"