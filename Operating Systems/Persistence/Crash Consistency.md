# Explanation
- *Crash Consistency Problem*: "Imagine you have to update two on-disk structures, A and B, in order to complete a particular operation. Because the disk only services a single request at a time, one of these requests will reach the disk first (either A or B). If the system crashes or loses power after one write completes, the on-disk structures will be left in an inconsistent state."

## Solution 1: File System Checker

### Description
- Let inconsistencies happen and then fix them later when rebooting.
- `fsck`: UNIX tool for finding such inconsistencies and repairing them.

### Mechanism
- When booting, run the file system checker to inspect any inconsistencies within the file system and fix them.

### Advantages
- Makes sure file system metadata is internally consistent.

### Disadvantages
- Can't fix all problems
	- e.g, file system looks consistent but the #inode points to garbage data.
- Too slow
	- i.e, with a very large disk volume, scanning the entire disk can take many minutes or even hours.

## Solution 2: Journaling (AKA Write-Ahead Logging)

### Description
- It's the most popular solution to the crash consistency problem and is borrowed from the world of database management systems.
- The basic idea:
	- When writing to disk, before overwriting the structures in place, first write down a little note describing what you are about to do.
	- If a crash occurs during an update, you can go back and look at the note you made and try again.
- A file system that uses journaling has the same on-disk layout as other regular file systems. Illustration: ![[Journaling File System On-disk Layout.png]]
	- The journal Structure: ![[Journal Structure.png]]
		- TxB: contains info about the pending update and a transaction identifier.
		- The blocks in-between are the blocks updated.
		- TxE: marker of the end of this transaction and will also contain the transaction identifier.

### Mechanism
- Main Mechanism:
	1. **Journal Write**: write the transaction to the log and wait for the write to complete.
	2. **Checkpoint**: Write the pending metadata and data updates to their final locations in the file system.
- Structure of data and metadata blocks:
	- *Physical Logging*: putting the exact physical contents of the update.
	- *Logical Logging*: putting a more compact logical representation of the update.
- How to write the transaction to the log?
	- Issue on write at a time, waiting for each one to complete and then issuing the next.
		- Advantages: Safe
		- Disadvantages: slow
	- Issue all the writes at once to the disk, turning all writes into a big sequential write to the disk.
		- Advantages: fast
		- Disadvantages: unsafe since the disk might reschedule them internally and complete them in any order.
	-  Issue the transaction in two steps: 1) write all blocks except TxE 2) write TxE
		- Advantages:
			- fast since any transaction can be written to the disk in only two IO requests.
			- Safe: if a crash happens during the update, it will be easy to recover.
		- Disadvantages:
		- Other Concerns: the TxE block size should be less than 512 bytes to guarantee its atomicity.
- **Recovery Mechanism**:
	- If the crash happens before the transaction is written safely to the log, the pending update is skipped since we don't have much info about how to recover it.
	- If the crash happens after the transaction has been committed to the log, the system can recover the update by redoing the update. (AKA #redo_logging)

### Advantages
- Solves the crash consistency problem.

### Disadvantages
- Adds a little overhead to each disk write.

# Sources
- OSTEP Chapter 42 - "Crash Consistency: FSCK and Journaling"