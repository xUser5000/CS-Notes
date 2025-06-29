# Explanation
- PostgreSQL is a client/server type relational database management system with a multi-process architecture that runs on a single host.
	- It uses [[Concurrent Query Execution#Process per Worker|process-per-worker]] model.

## Process Types
- A collection of multiple [[Process|processes]] that cooperatively manage a database cluster is usually referred to as a ‘PostgreSQL server’. It contains the following types of processes:
	- The **postgres server process** is the parent of all processes related to database cluster management.
	- Each **backend process** handles all queries and statements issued by a connected client.
	- Various **background processes** perform processes perform tasks for database management, such as VACUUM and CHECKPOINT processing.
	- **Replication-associated processes** perform streaming replication.
	- **Background worker processes** supported from version 9.3, it can perform any processing implemented by users. For more information, refer to the [official document](http://www.postgresql.org/docs/current/static/bgworker.html).

### Server Process
- In earlier versions, it was referred to as _postmaster_.
- It allocates a shared memory area, initiates various background processes, starts replication-related processes and background worker processes as needed, and waits for connection requests from clients.
- When a connection request is received, it spawns a [[#Backend Process]] to handle the client session.
- A postgres server process listens to one network port, the default port is 5432.

### Backend Process
- A backend process, also called a _postgres_ process, is started by the postgres server process and handles all queries issued by one connected client.
- It communicates with the client using a single [[TCP (Transmission Control Protocol)|TCP]] connection and terminates when the client disconnects.
- Each backend process can only operate on one database at a time, requiring explicit database selection during connection.
- PostgreSQL supports concurrent connections from multiple clients, with the maximum number controlled by the [max_connections](http://www.postgresql.org/docs/current/static/runtime-config-connection.html#GUC-MAX-CONNECTIONS) configuration parameter (default: 100).
- If many clients, such as WEB applications, frequently connect and disconnect from a PostgreSQL server, it can increase the cost of establishing connections and creating backend processes, as PostgreSQL does not have a native connection pooling feature.
	- This can have a negative impact on the performance of the database server.
	- To deal with such a case, a connection pooling middleware such as [pgbouncer](https://pgbouncer.github.io/) or [pgpool-II](http://www.pgpool.net/mediawiki/index.php/Main_Page) is usually used.

### Background Process
| process                    | description                                                                                                                                                                                                                                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| background writer          | This process writes dirty pages on the shared [[Database Buffer Pool\|buffer pool]] to a persistent storage (e.g., [[Hard Disk Drives (HDD)]], [[Solid State Drives (SSD)\|SSD]]) on a regular basis gradually. (In versions 9.1 or earlier, it was also responsible for the checkpoint process.) |
| checkpointer               | This process performs the checkpoint process in versions 9.2 or later.                                                                                                                                                                                                                            |
| autovacuum launcher        | This process periodically invokes the autovacuum-worker processes for the vacuum process. (More precisely, it requests the postgres server to create the autovacuum workers.)                                                                                                                     |
| WAL writer                 | This process writes and flushes the [[Crash Recovery#Write-Ahead Logging\|WAL]] data on the WAL buffer to persistent storage periodically.                                                                                                                                                        |
| WAL Summarizer             | This process tracks changes to all database blocks and writes these modifications to WAL summary files. Introduced in version 17.                                                                                                                                                                 |
| statistics collector       | This process collects statistics information such as for pg_stat_activity and pg_stat_database, etc.                                                                                                                                                                                              |
| logging collector (logger) | This process writes error messages into log files.                                                                                                                                                                                                                                                |
| io worker                  | These processes handle read operations asynchronously, offloading I/O tasks from regular backend processes. (Asynchronous I/O has been introduced since version 18 to improve I/O performance.)                                                                                                   |
| archiver                   | This process executes archiving logging.                                                                                                                                                                                                                                                          |


## Illustration
![[An example of the process architecture in PostgreSQL.png]]


# Sources
- [InterDB - 2.1 Process Architecture](https://www.interdb.jp/pg/pgsql02/01.html)
