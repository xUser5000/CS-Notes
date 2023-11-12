# Explanation
- Parallel execution provides a number of key benefits for DBMS:
	- Increased performance in throughput (more queries per second) and latency (less time per query).
	- Increased responsiveness and availability from the perspective of external clients of the DBMS.
- *Parallel DBMS*: a DBMS in which resources, or nodes, are physically close to each other.
	- Nodes communicate with high-speed interconnect.
	- Communication between resources is fast, cheap, and reliable.
- *Distributed DBMS*: a DBMS in which resources may be far away from each other.
	- Resources communicate using a slower interconnect over a public network.
	- Communication costs between nodes are higher and failures cannot be ignored.
- Even though a database may be physically divided over multiple resources, it still appears as a single logical database instance to the application.
	- A SQL query executed against a single-node DBMS should generate the same result on a parallel or distributed DBMS.

## Process Models
- DBMS process model defines how the system supports concurrent requests from a multi-user application/environment.
- The DBMS is comprised of more or more workers that are responsible for executing tasks on behalf of the client and returning the results.
	- An application may send a large request or multiple requests at the same time that must be divided across different workers.

### Process per Worker
#### Description
- A process model in which each worker is a separate OS process, and thus relies on OS scheduler.
	- Relying on the OS for scheduling effectively reduces the DBMS’s control over execution.
- Depends on shared memory or message passing to maintain global data structures. 
- Examples of systems that utilize the process-per-worker process model include: IBM DB2, Postgres, and Oracle.
	- When these DBMSs were developed, pthreads had not yet become the standard threading model.
	- The semantics of threading varied from OS to OS while `fork()` was better defined.

#### Illustration
![[Illustration of Process per Worker Model.png]]
#### Advantages
- A process crash doesn’t disrupt the whole system because each worker runs in the context of its own OS process.
#### Disadvantages
- Multiple workers on separate processes potentially make numerous copies of the same page.
	- The solution is to use shared-memory for global data structures.

### Thread per Worker Model
#### Description
- A process model in which the DBMS has only one process with multiple worker threads. 
- It's the most common model nowadays.
- There may or may not be a dispatcher thread.
- Almost every DBMS created in the last 20 years uses this approach, including Microsoft SQL Server and MySQL.
	- IBM DB2 and Oracle have updated their models to provide support for this apporach, as well.
- Postgres and Postgres-dervied databases largely still use the process-based approach.
#### Illustration
![[Thread per Worker Model.png]]
#### Advantages
- Extends [[Thread#Benefits]].
- The DBMS has full control over the tasks and threads and can manage its own scheduling.
- There is less overhead per context switch.
- A shared model does not have to be maintained.
#### Disadvantages
- 

### Embedded DBMS
#### Description
- A very different usage pattern for databases involves running the system in the same address space of the application.
	- This is opposite to a client-server model where the database stands independent of the application. 
- In this scenario, the application will set up the threads and tasks to run on the database system.
- The application itself will largely be responsible for scheduling.
- DuckDB, SQLite, and RocksDB are the most famous embedded DBMSs.
#### Illustration
![[Embedded DBMS.png]]
#### Advantages
- 
#### Disadvantages
-


## Query Parallelism

### Inter-Query Parallelism
- In inter-query parallelism, the DBMS executes different queries concurrently.
- If the queries are read-only, then little coordination is required between queries.
- If multiple queries are updating the database concurrently, more complicated conflicts arise.
	- See [[Concurrency Control]].

### Intra-Query Parallelism
- It's a type of parallel query execution in which the DBMS executes the operations of a single query in parallel, decreasing latency for long-running queries.
- The organization of intra-query parallelism can be thought of in terms of a producer/consumer paradigm:
	- Each operator is a producer of data as well as a consumer of data from some operator running below it.
- Parallel algorithms exist for every relational operator.
- The DBMS can either have multiple threads access centralized data structures or use partitioning to divide work up.
- Within intra-query parallelism, there are three types of parallelism: intra-operator, inter-operator, and bushy.
	- It is the DBMS’ responsibility to combine these techniques in a way that optimizes performance on a given workload.

#### Intra-Operator Parallelism (Horizontal)
- In intra-operator parallelism, the query plan’s operators are decomposed into independent fragments that perform the same function on different (disjoint) subsets of data.
- The DBMS inserts an *exchange operator* into the query plan to coalesce results from child operators.
	- Exchange operator prevents the DBMS from executing operators above it in the plan until it receives all of the data from the children.
- There are three types of exchange operators:
	- **Gather**: Combine the results from multiple workers into a single output stream.
		- This is the most common type used in parallel DBMSs.
	- **Distribute**: Split a single input stream into multiple output streams.
	- **Repartition**: Reorganize multiple input streams across multiple output streams. 
		- Allows the DBMS take inputs that are partitioned one way and then redistribute them in another way.
- Illustration: ![[Intra-Operator Parallelism.png]]

#### Inter-Operator Parallelism
- In inter-operator parallelism, the DBMS overlaps operators in order to pipeline data from one stage to the next without materialization.
- This approach is widely used in stream processing systems, which are systems that continually execute a query over a stream of input tuples.
- Illustration: ![[Inter-Operator Parallelism.png]]


## IO Parallelism
- To get around this, DBMSs use I/O parallelism to split installation across multiple devices.
- Two approaches to I/O parallelism are as follows:

### Multi-Disk Parallelism
- In multi-disk parallelism, The OS/hardware is configured to store the DBMS’s files across multiple storage devices.
	- This can be done through storage appliances or RAID configuration.
- All of the storage setup is transparent to the DBMS.

### Database Partitioning
- In database partitioning, the database is split up into disjoint subsets that can be assigned to discrete disks.

# Sources
- CMU 15-445 Lecture 13 - "Query Processing 2"