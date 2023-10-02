# Explanation
- A way to lay out pages on disk (refer to [[Database Storage#Page Layout]]).
- Similar to [[Log-structured File System (LFS)]].
- Instead of storing tuples, the DBMS only stores log records.
	- Log records contains information about how the database was modified (put and delete).
	- Each log record contains the tuple’s unique identifier.
- To read a record, the DBMS scans the log file backwards from newest to oldest and “recreates” the tuple. 
- To avoid long reads, the DBMS can have indexes to allow it to jump to specific locations in the log.
- It periodically compacts the log.
	- e.g, If it had a tuple and then made an update to it, it could compact it down to just inserting the updated tuple.
	- The database can compact the log into a table sorted by the id since the temporal information is not needed anymore. These are called Sorted String Tables (SSTables) and they can make the tuple search very fast.

## Advantages
- Fast writes.
	- Disk writes are sequential and existing pages are immutable which leads to reduced random disk I/O.
- Works well on append-only storage because the DBMS cannot go back and update the data.

## Disadvantages
- Potentially slow reads.
- Compaction makes the DBMS end up with write amplification. (It re-writes the same data over and over again.)

# Sources
- CMU 15-445 Lecture 4 - "Database Storage (Part II)"