# Explanation
- *Timestamp Ordering*: an #optimistic class of concurrency control protocols where the DBMS uses timestamps to determine the serializability order of transactions.
- Each transaction $T_i$ is assigned a **unique** fixed timestamp $TS(T_i)$ that is monotonically increasing.
- If $TS(T_i) \lt TS(T_j)$, then the DBMS must ensure that the execution schedule is equivalent to the serial schedule where $T_i$ appears before $T_j$.

## Mechanism
- Every database object X is tagged with timestamp of the last transaction that successfully performed a read ($R-TS(X)$) or write ($W-TS(X)$) on that object.
- If a transaction tries to access an object in a way that violates the timestamp ordering, the transaction is aborted and restarted.

### Read Operations
- If $TS(T_i) \lt W-TS(X)$,
	- this violates the timestamp order of $T_i$ with regard to the previous write of $X$ since $T_i$ would read something written in the future.
	-  $T_i$ must be restarted with a new timestamp.
- Otherwise, the read is valid
	- The DBMS updates $R-TS(X) := max(R-TS(X), TS(T_i))$
	- $T_i$ makes a local copy of $X$ to ensure repeatable reads.

### Write Operations
- If $TS(T_i) \lt R-TS(X)$ or $TS(T_i) \lt W-TS(X)$,
	- this violates the timestamp ordering since $T_i$ would override future changes.
	- $T_i$ must be restarted with a new timestamp.
- Otherwise, the write is valid
	- The DBMS updates $W-TS(X)$
	- $T_i$ makes a local copy of $X$ to ensure repeatable reads.

### Thomas Write Rule
- If $T_i$ wants to update $X$ where $TS(T_i) \lt W-TS(X)$, the DBMS can instead ignore the write and allow the transaction to continue instead of aborting and restarting it.
- This violates timestamp order of $T_i$, but it's okay because no other transaction will ever read $T_i$'s write to $X$.

## Advantages
- Generates a schedule that is conflict serializable if it doesn't use [[#Thomas Write Rule]].
- [[Deadlock|Deadlocks]] are not allowed by nature because no transaction ever waits.

## Disadvantages
- There is a possibility of starvation for long transactions if short transactions keep causing conflicts.
- Permits schedules that are not [[Concurrency Control#^11d00b|recoverable]].
- High overhead from copying data to transaction’s workspace and from updating timestamps.
- Suffers from the timestamp allocation bottleneck on highly concurrent systems.

# Sources
- CMU 15-445 Lecture 17 - "Timestamp Ordering Concurrency Control"