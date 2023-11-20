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
		- #pessimistic: assumes the transactions will conflict, so it doesn't let problems arise in the first place. ^68cb0f
		- #optimistic: assumes that conflicts are rare, so it chooses to deal with conflicts when they happen after the transactions commit.
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
	- **Read-Write Conflicts** ( #unrepeatable_read): A transaction is not able to get the same value when reading the same object multiple times.
	- **Write-Read Conflicts** ( #dirty_read): A transaction sees the write effects of a different transaction before that transaction committed its changes.
	- **Write-Write conflict** ( #lost_update): One transaction overwrites the uncommitted data of another concurrent transaction.

### Transaction Locks [2]
- Extends [[Database Systems/Database Storage/Data Structures/Concurrency#Lock]]
- A DBMS uses locks to dynamically generate execution schedules that is serializable without knowing each transaction's read/write set ahead of time.
- *Lock Manager*: a module inside the DBMS that:
	- decides whether a transaction can acquire a lock or not, and
	- provides a global view of what's going on inside the system.
- Basic types of locks:
	- **Shared Lock** (S): allows multiple transactions to read the same object at the same time.
		- If one transaction holds a shared lock, then another transaction can also acquire that same shared lock.
	- **Exclusive Lock** (X): allows a transaction to modify an object.
		- prevents other transactions from taking any other locks.
- Locks need to be complemented by a concurrency control protocol resolve issues associated with concurrent transactions.

#### Lock Granularities [2]
- The DBMS can use a lock hierarchy that allows a transaction to take more coarse-grained locks. Example:
	1. Database level (Slightly Rare)
	2. Table level (Very Common)
	3. Page level (Common)
	4. Tuple level (Very Common)
	5. Attribute level (Rare)
- Intention locks allow a higher level node to be locked in shared mode or exclusive mode without having to check all descendant nodes.
	- If a lock is in intention mode, then explicit locking is being done at a lower level in the tree.
- Additional types of locks:
	- **Intention-Shared** (IS): indicates explicit locking at a lower level with shared locks.
	- **Intention-Exclusive** (IX): Indicates explicit locking at a lower level with exclusive or shared locks.
	- **Shared+Intention-Exclusive** (SIX): The sub-tree rooted at that node is locked explicitly in shared mode and explicit locking is being done at a lower level with exclusive-mode locks.

### Examples
- [[Two-Phase Locking]]

# Sources
1. CMU 15-445 Lecture 15 - "Concurrency Control Theory"
2. CMU 15-445 Lecture 16 - "Two-Phase Locking"