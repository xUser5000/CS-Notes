# Explanation

## Introduction

### Transaction Locks
- Extends [[Database Systems/Database Storage/Data Structures/Concurrency#Lock]]
- A DBMS uses locks to dynamically generate execution schedules that are serializable without knowing each [[Concurrency Control#^68a44d|transaction]]'s read/write set ahead of time.
- *Lock Manager*: a module inside the DBMS that:
	- decides whether a transaction can acquire a lock or not, and
	- provides a global view of what's going on inside the system.
- Basic types of locks:
	- **Shared Lock** (S): allows multiple transactions to read the same object at the same time.
		- If one transaction holds a shared lock, then another transaction can also acquire that same shared lock.
	- **Exclusive Lock** (X): allows a transaction to modify an object.
		- prevents other transactions from taking any other locks.
- Locks need to be complemented by a [[Concurrency Control Protocols|concurrency control protocol]] resolve issues associated with concurrent transactions.

### Lock Granularities
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


## Traditional Two-Phase Locking
### Description
- *Two-Phase Locking*: #pessimistic [[Concurrency Control Protocols|concurrency control protocol]] that uses locks to determine whether a transaction is allowed to access an object in the database on the fly.
	- The protocol doesn't need to know all of the queries that a transaction will execute ahead of time.
- Phases:
	- **Growing**: each transaction requests the locks that it needs from the lock manager, which grants/denies these lock requests.
	- **Shrinking**: transactions enter this phase immediately after it releases its first lock.
		- transactions in this phase are only allowed to release locks,
		- they are not allowed to acquire new locks.

### Advantages
- Guarantees conflict serializability (generates schedules whose precedence graph is acyclic).
### Disadvantages
- *Cascading Aborts*: a case when a transaction aborts and now another transaction must be rolled back, which results in wasted work.
- #dirty_read
- [[Deadlock]]

## Strong Strict Two-Phase Locking
### Description
- *Strict Schedule*: a schedule where any value by a transaction is never read or overwritten by another transaction until the first transaction commits.
- *Strong Strict 2PL*: a variant of  2PL where transactions only release locks when they commit.
### Advantages
- Extends [[#Traditional Two-Phase Locking#Advantages]].
- Prevents cascading aborts.
### Disadvantages
- Generates more #pessimistic schedules that limit concurrency.

## Deadlock
- Extends [[Deadlock#Explanation]].
- Extends [[Deadlock#Conditions [1|Deadlock Conditions]].

### Prevention
- Extends [[Deadlock#Prevention [1|Deadlock Prevention]].
- When a transaction tries to acquire a lock held by another transaction (which could cause a deadlock), the DBMS kills one of them.
- Transactions are assigned priorities based on timestamps (older transactions have higher priorities).
- There are two ways to kill transactions under deadlock prevention:
	- **Wait-Die** (Old waits for Young): If the requesting transaction has a higher priority than the holding transaction, it waits.
		- Otherwise, it aborts.
	- **Wound-Wait** (Young Waits for Old): If the requesting transaction has a higher priority than the holding transaction, the holding transaction aborts and releases the lock.
		- Otherwise, the requesting transaction waits.

### Recovery
- Extends [[Deadlock#Recovery [1|Deadlock Recovery]].
- The DBMS creates a waits-for graphs where transactions are node,s and there exits a directed edge from $T_i$ to $T_j$ if transaction $T_i$ is waiting for transaction $T_j$ to release a lock.
- The DBMS will periodically check for cycles in the wait-for graph and then make a decision on how to break it.
- When the DBMS detects a deadlock, it will select a "victim" transaction to abort to break the cycle.
	- The victim transaction will either restart of abort depending on how the application invoked it.
- Properties to consider when selecting a victim transaction:
	- Age (timestamp)
	- Progress
	- no. items already locked
	- no. transactions needed to rollback with it
	- no. times a transaction has been restarted in the past.

# Sources
- CMU 15-445 Lecture 16 - "Two-Phase Locking"