# Explanation
- *ACID*: an acronym that describes the criteria used to ensure the correctness of database [[Concurrency Control#^68a44d|transactions]]. [1]

## Atomicity
- A transaction either executes all its actions or none of them. [1]
- In the context of ACID, atomicity is not about concurrency. [2]
	- It does not describe what happens if several processes try to access the same data at the same time, because that is covered under the letter I, for [[#Isolation]].
- There are two approaches to this: [1]
	- **Logging**
		- The DBMS logs all actions so that it can undo the actions of aborted transactions.
		- It maintains undo records both in memory and on disk.
		- Similar to [[Crash Consistency#Solution 2 Journaling (AKA Write-Ahead Logging)]].
	- **Shadow Paging**:
		- The DBMS makes copies of pages modified by the transactions.
		- Only when the transaction commits is the page made visible.
		- Has slower runtime performance than logging.
			- Rarely used in practice due to that.

## Consistency
- *Consistency*: an application-specific notion of the database being in a "good state". [2]
- [[#Atomicity]], [[#Isolation|isolation]], and [[#Durability|durability]] are properties of the database, whereas consistency (in the ACID sense) is a property of the application. [2]
- Notions: [1]
	- **Database Consistency**: The database accurately represents the real world entity it is modeling and follows integrity constraints.
		- Future transactions should see the effects of transactions in the past.
	- **Transaction Consistency**: If the database is consistent before the transaction starts, it will be consistent after.
		- This is the applications' responsibility,

## Isolation
- *Isolation*: the notion that concurrently executing transactions are isolated from each other: they cannot step on each other’s toes. [2]
- The DBMS provides transactions the illusion that they are running alone in the system such that they don't see the effects of concurrent transactions. [1]
- This is equivalent to a system where transactions are executed in serial order. [1]
- To achieve better performance the DBMS, interleaves the operations of concurrent transactions. [1]

## Durability
- *Durability*: the promise that once a transaction has committed successfully, any data it has written will not be forgotten, even if there is a hardware fault or the database crashes. [2]
- In a single-node database, durability typically means that the data has been written to nonvolatile storage such as a hard drive or SSD. [2]
	- It usually also involves a write-ahead log or similar which allows recovery in the event that the data structures on disk are corrupted.
- In a replicated database, durability may mean that the data has been successfully copied to some number of nodes. [2]

# Sources
1. CMU 15-445 Lecture 15 - "Concurrency Control Theory"
2. Designing Data-Intensive Applications - Chapter 7
