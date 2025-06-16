# Explanation
- Memory architecture in PostgreSQL can be classified into two broad categories:
	- Local memory area – allocated by each backend process for its own use.
	- Shared memory area – used by all processes of a PostgreSQL server.

## Local Memory Area
- Each [[Process Architecture#Backend Process|backend process]] allocates a local memory area for query processing.
- The area is divided into several sub-areas, whose sizes are either fixed or variable:

| sub-area             | description                                                                                                                                                                                                                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| work_mem             | The [[Query Execution\|executor]] uses this area for sorting tuples by ORDER BY and DISTINCT operations, and for [[Joins\|joining]] tables by [[Joins#Sort-Merge Join\|merge-join]] and [[Joins#Hash Join\|hash-join]] operations. |
| maintenance_work_mem | Some kinds of maintenance operations (e.g., VACUUM, REINDEX) use this area.                                                                                                                                                        |
| temp_buffers         | The executor uses this area for storing temporary tables.                                                                                                                                                                          |

## Shared Memory Area
A shared memory area is allocated by a [[Process Architecture#Server Process|PostgreSQL server]] when it starts up.
This area is also divided into several fixed-sized sub-areas:

| sub-area           | description                                                                                                                                                                                                                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| shared buffer pool | PostgreSQL loads pages within tables and [[Database Systems/Database Storage/Data Structures/Data Structures\|indexes]] from a persistent storage to this area, and operates them directly.                                                                                                                           |
| WAL buffer         | To ensure that no data has been lost by server failures, PostgreSQL supports the [[Crash Recovery#Write-Ahead Logging\|WAL]] mechanism. WAL data (also referred to as XLOG records) are the transaction log in PostgreSQL. The WAL buffer is a buffering area of the WAL data before writing to a persistent storage. |
| commit log         | The commit log (CLOG) keeps the states of all transactions (e.g., in_progress, committed, aborted) for the concurrency control (CC) mechanism.                                                                                                                                                                        |

- In addition to the shared buffer pool, WAL buffer, and commit log, PostgreSQL allocates several other areas:
	- Sub-areas for the various access control mechanisms. (e.g., [[Semaphore|semaphores]], lightweight [[Lock|locks]], [[Two-Phase Locking#Lock Granularities|shared and exclusive locks]], etc)
	- Sub-areas for the various background processes, such as the checkpointer and autovacuum.
	- Sub-areas for [[Concurrency Control#^68a44d|transaction]] processing, such as savepoints and two-phase commit.

## Illustration
![[Memory Architecture in PostgreSQL.png]]

# Sources
- [InterDB - 2.2 Memory Architecture](https://www.interdb.jp/pg/pgsql02/02.html)
