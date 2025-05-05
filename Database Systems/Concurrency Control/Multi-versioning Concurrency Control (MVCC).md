# Explanation
- Multi-Version Concurrency Control (MVCC) is a larger concept than just a concurrency control protocol. [1]
	- It involves all aspects of the DBMS’s design and implementation.
- It's the most widely used scheme in DBMSs. [1]

## Mechanism [1]
- The DBMS maintains multiple physical versions of a single logical object in the database.
- When a transaction writes to an object, the DBMS creates a new version of that object.
- When a transaction reads an object, it reads the newest version that existed when the transaction started.

### Concurrency Control Protocol [1]
- Any protocol mentioned in [[Concurrency Control#Examples]] cab be used with MVCC.

### Version Storage [1]
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

### Garbage Collection [1]
- The DBMS needs to remove reclaimable physical versions from the database over time.
- A version is re claimable if no active transaction can “see” that version or if it was created by a transaction that was aborted.

#### Tuple-level Garbage Collection [1]
- The DBMS finds old versions by examining tuples directly.
- There are two approaches to achieve this:
	- **Background Vacuuming**: Separate threads periodically scan the table and look for reclaimable versions.
		- This works with any version storage scheme.
		- A simple optimization is to maintain a “dirty page bitmap,” which keeps track of which pages have been modified since the last scan.
			- This allows the threads to skip pages which have not changed.
	- **Cooperative Cleaning**: Worker threads identify reclaimable versions as they traverse version chain.
		- This only works with O2N chains.

#### Transaction-level Garbage Collection [1]
- Each transaction is responsible for keeping track of their own old versions so the DBMS does not have to scan tuples.
- Each transaction maintains its own read/write set.
- When a transaction completes, the garbage collector can use that to identify which tuples to reclaim.
- The DBMS determines when all versions created by a finished transaction are no longer visible.

### Index Management [2]
- How do indexes work in a multi-version database?

#### Approach 1
- One option is to have the index simply point to all versions of an object and require an index query to filter out any object versions that are not visible to the current transaction.
- When [[#Garbage Collection|garbage collection]] removes old object versions that are no longer visible to any transaction, the corresponding index entries can also be removed.

#### Approach 2
- Another option would be to use an append-only/copy-on-write variant of [[B+ Tree|B-trees]] that does not overwrite pages of the tree when they are updated, but instead creates a new copy of each modified page.
	- Parent pages, up to the root of the tree, are copied and updated to point to the new versions of their child pages.
	- Any pages that are not affected by a write do not need to be copied, and remain immutable.
- With append-only B-trees, every write transaction (or batch of transactions) creates a new B-tree root, and a particular root is a consistent snapshot of the database at the point in time when it was created.
- There is no need to filter out objects based on transaction IDs because subsequent writes cannot modify an existing B-tree; they can only create new tree roots.
- This approach requires a background process for compaction and garbage collection.

## Advantages [1]
- Writers do not block writers and readers do not block readers.
- Read-only transactions can read a consistent snapshot of the database without using locks of any kind.
- Multi-versioned DBMSs can easily support time-travel queries, which are queries based on the state of the database at some other point in time.

## Disadvantages


# Sources
1. CMU 15-445 Lecture 18 - "Multi-versioning Concurrency Control"
2. Designing Data-Intensive Applications - Chapter 7
