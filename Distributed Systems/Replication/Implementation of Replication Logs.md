# Explanation

## Statement-based replication
- **Description**: the leader logs every write request (statement) that it executes and sends that statement log to its followers.
- **Advantages**:
	- Simple.
- **Disadvantages**:
	- Any statement that calls a nondeterministic function, such as `NOW()` to get the current date and time or `RAND()` to get a random number, is likely to generate a different value on each replica.
		- The leader can replace any nondeterministic function calls with a fixed return value when the statement is logged so that the followers all get the same value.
	- If statements use an auto-incrementing column, or if they depend on the existing data in the database, they must be executed in exactly the same order on each replica, or else they may have a different effect.
	- Statements that have side effects may result in different side effects occurring on each replica, unless the side effects are absolutely deterministic.

## Write-ahead log (WAL) shipping
- **Description**:
	- The [[Crash Recovery#Write-Ahead Logging|log]] can be used to build a replica on another node: besides writing the log to disk, the leader also sends it across the network to its followers.
	- When the follower processes this log, it builds a copy of the exact same data structures as found on the leader.
- **Advantages**:
- **Disadvantages**:
	- The log describes the data on a very low level, which makes replication closely coupled to the storage engine.
		- If the database changes its storage format from one version to another, it is typically not possible to run different versions of the database software on the leader and the followers.

## Logical (row-based) log replication
- **Description**: Use different log formats for replication and for the storage engine, which allows the replication log to be decoupled from the storage engine internals.
	- A logical log for a relational database is usually a sequence of records describing writes to database tables at the granularity of a row
- **Advantages**:
	- The logical log can more easily be kept backward compatible, allowing the leader and the follower to run different versions of the database software, or even different storage engines.
	- Easier for external applications to parse, which is useful if we want to send the contents of a database to an external system.
- **Disadvantages**:
	- The log describes the data on a very low level, which makes replication closely coupled to the storage engine.
		- If the database changes its storage format from one version to another, it is typically not possible to run different versions of the database software on the leader and the followers.

## Trigger-based replication
// TODO

# Sources
- Designing Data-Intensive Applications - Chapter 5
