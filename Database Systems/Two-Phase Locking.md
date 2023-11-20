# Explanation

## Traditional Two-Phase Locking
### Description
- *Two-Phase Locking*: #pessimistic concurrency control protocol that uses locks to determine whether a transaction is allowed to access an object in the database on the fly.
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