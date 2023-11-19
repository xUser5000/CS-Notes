# Explanation
- *Transaction*: execution of one or more operations (e.g, SQL queries) on a shared database to perform some higher level function.
	- Basic unit of change in DBMS.
	- Must be #atomic.
- Transaction starts with the BEGIN command and ends with either COMMIT  or ABORT.
	- On ABORT, all modifications done by a transaction is rolled back as if the transaction never happened in the first place.

## ACID
- It's an acronym that describes the criteria used to ensure the correctness of database transactions.

### Atomicity
- A transaction either executes all its actions or none of them.
- There are two approaches to this:
	- **Logging**
		- The DBMS logs all actions so that it can undo the actions of aborted transactions.
		- It maintains undo records both in memory and on disk.
		- Similar to [[Crash Consistency#Solution 2 Journaling (AKA Write-Ahead Logging)]].
	- **Shadow Paging**:
		- The DBMS makes copies of pages modified by the transactions.
		- Only when the transaction commits is the page made visible.
		- Has slower runtime performance than logging.
			- Rarely used in practice due to that.

### Consistency
- *Consistency*: means the world represented by the database is logically correct.
- Notions:
	- **Database Consistency**: The database accurately represents the real world entity it is modeling and follows integrity constraints.
		- Future transactions should see the effects of transactions in the past.
	- **Transaction Consistency**: If the database is consistent before the transaction starts, it will be consistent after.
		- This is the applications' responsibility,

### Isolation
- The DBMS provides transactions the illusion that they are running alone in the system such that they don't see the effects of concurrent transactions.
- This is equivalent to a system where transactions are executed in serial order.
- To achieve better performance the DBMS, interleaves the operations of concurrent transactions.

### Durability
- All of the changes committed by a transaction must be durable after a crash or restart.
- The DBMS can either use logging or shadow paging to ensure that.

## Concurrency Control Protocols
- *Concurrency Control Protocol*: describes how the DBMS decides the proper interleaving of operations from multiple transactions at runtime.
	- Categories of concurrency control protocols:
		- **Pessimistic**: assumes the transactions will conflict, so it doesn't let problems arise in the first place.
		- **Optimistic**: assumes that conflicts are rare, so it chooses to deal with conflicts when they happen after the transactions commit.
- *Execution Schedule*: the order in which the DBMS executes operations.
- *Serial Schedule*: a schedule that doesn't interleave the actions of different transactions.
- *Equivalent Schedules*: for any database state, if the effect of executing the first schedule is identical to the effects of executing the second schedule, the two schedules are equivalent.
- *Serializable Schedule*: a schedule which is equivalent to any serial execution of the transactions.
	- Different serial executions can produce different results, but all are considered correct.
- The goal of a concurrency control protocol is to generate an e execution schedule that is equivalent to some serial schedule.

###  Conflicts
- A conflict between two operations happens if:
	- the operations are for different transactions,
	- they are performed on the same object, and
	- at least one of the operations is a write.
- Three are three variations of conflicts:
	- **Read-Write Conflicts** (Unrepeatable Reads): A transaction is not able to get the same value when reading the same object multiple times.
	- **Write-Read Conflicts** (Dirty Reads): A transaction sees the write effects of a different transaction before that transaction committed its changes.
	- **Write-Write conflict** (Lost Updates): One transaction overwrites the uncommitted data of another concurrent transaction.

# Sources
- CMU 15-445 Lecture 15 - "Concurrency Control Theory"