# Explanation
- *Concurrency Control Protocol*: describes how the DBMS decides the proper interleaving of operations from multiple [[Concurrency Control#^68a44d|transactions]] at runtime.
	- Categories of concurrency control protocols:
		- #pessimistic: assumes the transactions will conflict, so it doesn't let problems arise in the first place. ^68cb0f
		- #optimistic: assumes that conflicts are rare, so it chooses to deal with conflicts when they happen after the transactions commit.
- *Execution Schedule*: the order in which the DBMS executes operations.
- *Serial Schedule*: a schedule that doesn't interleave the actions of different transactions. ^5d741a
- *Equivalent Schedules*: for any database state, if the effect of executing the first schedule is identical to the effects of executing the second schedule, the two schedules are equivalent.
- *Serializable Schedule*: a schedule which is equivalent to any serial execution of the transactions. ^ec3fa1
	- Different serial executions can produce different results, but all are considered correct.
- *Recoverable Schedule*: a schedule where transactions commit only after all transactions whose changes they read, commit. ^11d00b
- The goal of a concurrency control protocol is to generate an execution schedule that is equivalent to some serial schedule.

## Topics
- [[Transaction Anomalies]]
- [[Transaction Isolation Levels]]

## Examples
- [[Two-Phase Locking]]
- [[Timestamp Ordering]]
- [[Optimistic Concurrency Control (OCC)]]
- [[Multi-versioning Concurrency Control (MVCC)]]
- [[Serializable Snapshot Isolation (SSI)]]

# Sources
- CMU 15-445 Lecture 15 - "Concurrency Control Theory"
