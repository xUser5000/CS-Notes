# Explanation
- *Transaction*: execution of one or more operations (e.g, SQL queries) on a shared database to perform some higher level function. [1] ^68a44d
	- Basic unit of change in DBMS.
	- Must be #atomic.
- Transactions were created to simplify the programming model for applications accessing a database. [2]
- Transaction starts with the BEGIN command and ends with either COMMIT or ABORT. [1]
	- On ABORT, all modifications done by a transaction is rolled back as if the transaction never happened in the first place.

# Topics
- [[ACID]]
- [[Concurrency Control Protocols]]
	- [[Transaction Anomalies]]
	- [[Transaction Isolation Levels]]
	- Examples:
		- [[Two-Phase Locking]]
		- [[Timestamp Ordering]]
		- [[Optimistic Concurrency Control (OCC)]]
		- [[Multi-versioning Concurrency Control (MVCC)]]
		- [[Serializable Snapshot Isolation (SSI)]]

# Sources
1. CMU 15-445 Lecture 15 - "Concurrency Control Theory"
2. Designing Data-Intensive Applications - Chapter 7
