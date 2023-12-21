# Explanation
- Multi-Version Concurrency Control (MVCC) is a larger concept than just a concurrency control protocol.
	- It involves all aspects of the DBMS’s design and implementation.
- It's the most widely used scheme in DBMSs.

## Mechanism
- The DBMS maintains multiple physical versions of a single logical object in the database.
- When a transaction writes to an object, the DBMS creates a new version of that object.
- When a transaction reads an object, it reads the newest version that existed when the transaction started.

### Concurrency Control Protocol
- Any protocol mentioned in [[Concurrency Control#Examples]] cab be used with MVCC.

### Version Storage
- The DBMS uses the tuple’s pointer field to create a version chain per logical tuple.
	- which is essentially a linked list of versions sorted by timestamp.
	- This allows the DBMS to find the version that is visible to a particular transaction at runtime.
- Indexes always point to the “head” of the chain, which is either the newest or oldest version depending on implementation.
- A thread traverses chain until it finds the correct version.
- Different storage schemes determine where/what to store for each version.
	- **Append-Only Storage**:
		- All physical versions of a logical tuple are stored in the same table space.
		- Versions are mixed together in the table and each update just appends a new version of the tuple into the table and updates the version chain.
		- The chain can either be sorted oldest-to-newest (O2N) which requires chain traversal on look-ups, or newest-to-oldest (N2O), which requires updating index pointers for every new version.
	- **Time-Travel Storage**:
		- The DBMS maintains a separate table called the time-travel table which stores older versions of tuples.
		- On every update, the DBMS copies the old version of the tuple to the time-travel table and overwrites the tuple in the main table with the new data.
		- Pointers of tuples in the main table point to past versions in the time-travel table.
	- **Delta Storage**:
		- Like time-travel storage, but instead of the entire past tuples, the DBMS only stores the deltas, or changes between tuples in what is known as the delta storage segment.
		- Transactions can then recreate older versions by iterating through the deltas.
		- This results in faster writes than time-travel storage but slower reads.

### Garbage Collection
- The DBMS needs to remove reclaimable physical versions from the database over time.
- A version is re claimable if no active transaction can “see” that version or if it was created by a transaction that was aborted.

#### Tuple-level Garbage Collection
- The DBMS finds old versions by examining tuples directly.
- There are two approaches to achieve this:
	- **Background Vacuuming**: Separate threads periodically scan the table and look for reclaimable versions.
		- This works with any version storage scheme.
		- A simple optimization is to maintain a “dirty page bitmap,” which keeps track of which pages have been modified since the last scan.
			- This allows the threads to skip pages which have not changed.
	- **Cooperative Cleaning**: Worker threads identify reclaimable versions as they traverse version chain.
		- This only works with O2N chains.

#### Transaction-level Garbage Collection
- Each transaction is responsible for keeping track of their own old versions so the DBMS does not have to scan tuples.
- Each transaction maintains its own read/write set.
- When a transaction completes, the garbage collector can use that to identify which tuples to reclaim.
- The DBMS determines when all versions created by a finished transaction are no longer visible.

### Index Management
// TODO

## Advantages
- Writers do not block writers and readers do not block readers.
- Read-only transactions can read a consistent snapshot of the database without using locks of any kind.
- Multi-versioned DBMSs can easily support time-travel queries, which are queries based on the state of the database at some other point in time.

## Disadvantages


# Sources
- CMU 15-445 Lecture 18 - "Multi-versioning Concurrency Control"