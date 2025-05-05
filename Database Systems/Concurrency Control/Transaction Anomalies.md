# Explanation
- A conflict between two operations happens if: [2]
	- the operations are for different transactions/clients,
	- they are performed on the same object, and
	- at least one of the operations is a write.

## Dirty Reads
- One client reads another client’s writes before they have been committed. [1]

## Dirty Writes [1]
- One client overwrites data that another client has written, but not yet committed.

## Read Skew (non-repeatable reads) [1]
- A client sees different parts of the database at different points in time.

## Lost updates [1]
- Two clients concurrently perform a read-modify-write cycle. 
- One overwrites the other’s write without incorporating its changes, so data is lost.

## Write Skew [1]
- You can think of write skew as a generalization of the [[#Lost updates|lost update]] problem.
- Write skew can occur if two transactions read the same objects, and then update some of those objects (different transactions may update different objects).
	- In the special case where different transactions update the same object, you get a [[#Dirty Writes|dirty write]] or lost update anomaly (depending on the timing).
- **Scenario**:
	1. A transaction reads something,
	2. makes a decision based on the value it saw,
	3. and writes the decision to the database.
	4. However, by the time the write is made, the premise of the decision is no longer true.

## Phantom Reads [1]
- A transaction reads objects that match some search condition.
- Another client makes a write that affects the results of that search.

# Sources
1. Designing Data-Intensive Applications - Chapter 7
2. CMU 15-445 Lecture 15 - "Concurrency Control Theory"
