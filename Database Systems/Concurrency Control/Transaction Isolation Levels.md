# Explanation
- Isolation levels control the extent that a transaction is exposed to the actions of other concurrent transactions.

## Read Uncommitted
- A very weak isolation level where all [[Transaction Anomalies|anomalies]] can arise except for dirty writes.

## Read Committed

### Description
- *Read Committed*: the most basic level of transaction isolation. It makes two guarantees:
	1. When reading from the database, you will only see data that has been committed (no [[Transaction Anomalies#Dirty Reads|dirty reads]]).
	2. When writing to the database, you will only overwrite data that has been committed (no [[Transaction Anomalies#Dirty Writes|dirty writes]]).

### Implementation
- Databases typically prevent dirty writes by using row-level [[Two-Phase Locking#Transaction Locks|locks]]:
	1. when a transaction wants to modify a particular object (row or document), it must first acquire a lock on that object.
	2. It must then hold that lock until the transaction is committed or aborted. 
	3. Only one transaction can hold the lock for any given object;
	4. if another transaction wants to write to the same object, it must wait until the first transaction is committed or aborted before it can acquire the lock and continue.
- To prevent dirty reads we can use the same lock, and to require any transaction that wants to read an object to briefly acquire the lock and then release it again immediately after reading.
- This locking is done automatically by databases in read committed mode (or stronger isolation levels). 

### Potential Anomalies
- Phantoms
- Non-repeatable reads
- Lost updates

## Repeatable Reds
- Potential anomalies: phantoms.

## Snapshot Isolation

### Description
- The idea is that each transaction reads from a consistent snapshot of the database — that is, the transaction sees all the data that was committed in the database at the start of the transaction.
- Even if the data is subsequently changed by another transaction, each transaction sees only the old data from that particular point in time.
- From a performance point of view, a key principle of snapshot isolation is readers never block writers, and writers never block readers.
	- This allows a database to handle long-running read queries on a consistent snapshot at the same time as processing writes normally, without any lock contention between the two.

### Implementation
- Like read committed isolation, implementations of snapshot isolation typically use write [[Two-Phase Locking#Transaction Locks|locks]] to prevent dirty writes, which means that a transaction that makes a write can block the progress of another transaction that writes to the same object. 
- However, reads do not require any locks.
- Snapshot Isolation can be implemented using [[Multi-versioning Concurrency Control (MVCC)]].

### Potential Anomalies
- Write Skew

## Serializable

### Description
- *Serializable Isolation*: the database guarantees that transactions have the same effect as if they ran serially.
	- It's usually regarded as the strongest isolation level.
	- It prevents all possible anomalies/race conditions.
- In practice, serializable isolation has a performance cost, and many databases don’t want to pay that price.
	- It’s therefore common for systems to use weaker levels of isolation, which protect against some [[Transaction Anomalies|concurrency issues]], but not all.
	- Those levels of isolation are much harder to understand, and they can lead to subtle bugs, but they are nevertheless used in practice.

### Implementation
- [[Two-Phase Locking]]

### Potential anomalies
- None

## Default Isolation level in real systems
![[Default isolation levels in real systems.png]]

## Illustration
![[Illustration of isolation levels.png]]

# Sources
- Designing Data-Intensive Applications - Chapter 7
