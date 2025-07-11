# Explanation

## Abstract
- MapReduce is a programming model and an associated implementation for processing and generating large data sets.
- Users specify a map function that processes a key/value pair to generate a set of intermediate key/value pairs, and a reduce function that merges all intermediate values associated with the same intermediate key.
- Many real world tasks are expressible in this model, as shown in the paper.
- Programs written in this functional style are automatically parallelized and executed on a large cluster of commodity machines.
- The run-time system takes care of the details of partitioning the input data, scheduling the program’s execution across a set of machines, handling machine failures, and managing the required inter-machine communication.
- This allows programmers without any experience with parallel and distributed systems to easily utilize the resources of a large distributed system.

## Programming Model
- The computation takes a set of input key/value pairs, and produces a set of output key/value pairs.
- The user of the MapReduce library expresses the computation as two functions:
	- *Map*: takes an input pair and produces a set of intermediate key/value pairs.
		- The MapReduce library groups together all intermediate values associated with the same intermediate key I and passes them to the Reduce function.
	- *Reduce*: accepts an intermediate key I and a set of values for that key. It merges together these values to form a possibly smaller set of values.
		- Typically just zero or one output value is produced per Reduce invocation.
		- The intermediate values are supplied to the user’s reduce function via an iterator, which allows us to handle lists of values that are too large to fit in memory.

### Word Count Example
- Consider the problem of counting the number of occurrences of each word in a large collection of documents. The user would write code similar to the following pseudo-code:
```
map(String key, String value):
	// key: document name
	// value: document contents
	for each word w in value:
		EmitIntermediate(w, "1");

reduce(String key, Iterator values):
	// key: a word
	// values: a list of counts
	int result = 0;
	for each v in values:
		result += ParseInt(v);
	Emit(AsString(result));
```

### Types
- Even though the previous pseudo-code is written in terms of string inputs and outputs, conceptually the map and reduce functions supplied by the user have associated types:
```
map    (k1,v1)        -> list(k2,v2)
reduce (k2,list(v2))  -> list(v2)
```

### More Examples
- Distributed Grep.
- Count of URL Access Frequency.
- Reverse Web-Link Graph.
- Inverted Index.
- Distributed Sort.

## Implementation
- Many different implementations of the MapReduce interface are possible.
- The right choice depends on the environment.
	- For example, one implementation may be suitable for a small shared-memory machine, another for a large NUMA multi-processor, and yet another for an even larger collection of networked machines.

### Execution
- The Map invocations are distributed across multiple machines by automatically partitioning the input data into a set of $M$ splits.
	- The input splits can be processed in parallel by different machines.
- Reduce invocations are distributed by partitioning the intermediate key space into $R$ pieces using a partitioning function (e.g., $hash(key)$ mod $R$).
	- The number of partitions (R) and the partitioning function are specified by the user.
- Overall flow of a MapReduce operation:
	1. The MapReduce library in the user program first splits the input files into M pieces of typically 16 megabytes to 64 megabytes (MB) per piece (controllable by the user via an optional parameter). It then starts up many copies of the program on a cluster of machines.
	2. One of the copies of the program is special – the master. The rest are workers that are assigned work by the master. There are M map tasks and R reduce tasks to assign. The master picks idle workers and assigns each one a map task or a reduce task.
	3. 3. A worker who is assigned a map task reads the contents of the corresponding input split. It parses key/value pairs out of the input data and passes each pair to the user-defined Map function. The intermediate key/value pairs produced by the Map function are buffered in memory.
	4. Periodically, the buffered pairs are written to local disk, partitioned into R regions by the partitioning function. The locations of these buffered pairs on the local disk are passed back to the master, who is responsible for forwarding these locations to the reduce workers.
	5. When a reduce worker is notified by the master about these locations, it uses remote procedure calls to read the buffered data from the local disks of the map workers. When a reduce worker has read all intermediate data, it sorts it by the intermediate keys so that all occurrences of the same key are grouped together. The sorting is needed because typically many different keys map to the same reduce task. If the amount of intermediate data is too large to fit in memory, an external sort is used.
	6. The reduce worker iterates over the sorted intermediate data and for each unique intermediate key encountered, it passes the key and the corresponding set of intermediate values to the user’s Reduce function. The output of the Reduce function is appended to a final output file for this reduce partition.
	7. When all map tasks and reduce tasks have been completed, the master wakes up the user program. At this point, the MapReduce call in the user program returns back to the user code.
	8. After successful completion, the output of the mapreduce execution is available in the R output files (one per reduce task, with file names as specified by the user)

#### Illustration
![[MapReduce Execution overview.png]]

### Master Data Structures
- For each map task and reduce task, the master stores the state (idle, in-progress, or completed), and the identity of the worker machine (for non-idle tasks).

### Fault Tolerance

#### Worker Failure
- MapReduce is resilient to large-scale worker failures.
- The master pings every worker periodically.
- If no response is received from a worker in a certain amount of time, the master marks the worker as failed.
- Any map tasks completed by the worker are reset back to their initial idle state, and therefore become eligible for scheduling on other workers.
	- Similarly, any map task or reduce task in progress on a failed worker is also reset to idle and becomes eligible for rescheduling.
	- Completed map tasks are re-executed on a failure because their output is stored on the local disk(s) of the failed machine and is therefore inaccessible.
	- Completed reduce tasks do not need to be re-executed since their output is stored in a global file system.
- When a map task is executed first by worker A and then later executed by worker B (because A failed), all workers executing reduce tasks are notified of the re-execution.
- Any reduce task that has not already read the data from worker A will read the data from worker B.

#### Master Failure
- It is easy to make the master write periodic checkpoints of the master data structures described above.
	- If the master task dies, a new copy can be started from the last checkpointed state.
- However, given that there is only a single master, its failure is unlikely; therefore our current implementation aborts the MapReduce computation if the master fails.
	- Clients can check for this condition and retry the MapReduce operation if they desire.

### Locality
- We conserve network bandwidth by taking advantage of the fact that the input data (managed by GFS) is stored on the local disks of the machines that make up our cluster.
- The MapReduce master takes the location information of the input files into account and attempts to schedule a map task on a machine that contains a replica of the corresponding input data.
- Failing that, it attempts to schedule a map task near a replica of that task’s input data (e.g., on a worker machine that is on the same network switch as the machine containing the data).

### Task Granularity
- We subdivide the map phase into $M$ pieces and the reduce phase into $R$ pieces, as described above.
- Ideally, $M$ and $R$ should be much larger than the number of worker machines.
	- Having each worker perform many different tasks improves dynamic load balancing, and also speeds up recovery when a worker fails: the many map tasks it has completed can be spread out across all the other worker machines.
- There are practical bounds on how large $M$ and $R$ can be in our implementation, since the master must make $O(M + R)$ scheduling decisions and keeps $O(M \cdot R)$ state in memory as described above.

### Backup Tasks
- One of the common causes that lengthens the total time taken for a MapReduce operation is a “straggler”: a machine that takes an unusually long time to complete one of the last few map or reduce tasks in the computation.
- Reasons why stragglers can arise:
	- A machine with a bad disk may experience frequent correctable errors that slow its read performance from $30$ MB/s to $1$ MB/s.
	- The cluster scheduling system may have scheduled other tasks on the machine, causing it to execute the MapReduce code more slowly due to competition for CPU, memory, local disk, or network bandwidth.
- When a MapReduce operation is close to completion, the master schedules backup executions of the remaining in-progress tasks.
	- The task is marked as completed whenever either the primary or the backup execution completes.

# Sources
- [MapReduce: Simplified Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)
- [MIT 6.824 - Lecture 1](https://www.youtube.com/watch?v=cQP8WApzIQQ&list=PLrw6a1wE39_tb2fErI4-WkMbsvGQk9_UB&index=1&pp=iAQB)
