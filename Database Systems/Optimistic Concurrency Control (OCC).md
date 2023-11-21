# Explanation
- *Optimistic Concurrency Control Protocol*: a concurrency control protocol that uses timestamps (similar to [[Timestamp Ordering]]) to validate transactions.
- Works best when the number of conflicts is low, which happens when:
	- all transactions are read-only, or
	- when transactions access disjoint subsets of data.

## Mechanism
- The DBMS creates a private workspace for each transaction.
	- Any object read is copied into the workspace, and
	- any object written is copied to the workspace and modified there.
- No other transaction can read the changes made by another transaction in its private workspace.
- When a transaction commits, the DBMS compares the transaction's workspace *write set* to see whether it conflicts with other transactions or not.
	- If there are no conflicts, the write set is installed into the global database.
- OCC has three phases:
	- **Read Phase**: The DBMS tracks the read/write sets of transactions and stores their writes in a private workspace.
	- **Validation Phase**: When a transaction commits, the DBMS checks whether it conflicts with other transactions.
		- The DBMS assigns transactions timestamps in this phase.
		- To ensure serializability, the DBMS checks $T_i$ against other transactions for RW and WW conflicts and asserts all conflicts go in one way.
	- **Write Phase**: If validation succeeds, the DBMS applies the private workspace changes to the database.
		- Otherwise, it aborts and restarts the transaction.

## Advantages
- Generates Serializable schedules.

## Disadvantages
- High overhead for copying data locally into the transaction’s private workspace.
- Validation/Write phase bottlenecks.
- Aborts are potentially more wasteful than in other protocols because they only occur after a transaction has already executed.
- Suffers from timestamp allocation bottleneck.

# Sources
- CMU 15-445 Lecture 17 - "Timestamp Ordering Concurrency Control"