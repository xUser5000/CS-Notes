# Explanation
- It's a self-balancing tree data structure.
	- Does searching, sequential access, insertion, and deletion in $O(log N)$.
- Any DBMS that supports order-preserving indexes uses B+ trees.
- Stores data only in lead nodes while inner nodes serve as guide posts for search.

## Structure
- Parameter M represents the maximum number of children a node  can have.
- It is *perfectly* balanced (i.e, every leaf node is at the same depth).
- Every inner node other than the root is at least have full.
- Every inner node with k keys has k+1 non-null children.
- Every node in the tree contains an array of key-value pairs.
	- The array is almost always sorted.
	- The keys represent the attributes the index is based on.
	- The values of inner nodes are pointers to children nodes.
	- The values of leaf nodes can be record IDs or actual tuple data.

## Operations

### Insertion
```
1. Find correct leaf L.
2. Add a new entry into L in sorted order:
	- If L has enough space, we are done.
	- Otherwise:
		1. split L into two nodes L1 and L2.
		2. Redistribute entries evenly and copy up middle key.
		3. Insert index entry pointing to L2 in the parent of L1
3. To split an inner node, redistribute the entries evenly, but push up the middle key.
```

### Deletion
```
1. Find the correct leaf L.
2. Remove the entry:
	- If L is at leasy half full, we are done.
	- Otherwise,
		1. You can try to redistribute, borrowing from sibling.
		2. If distribution fails, merge L and sibling.
3. If merge occured, you must delete entry in parent pointing to L.
```

## Design Choices

### Node size
- Nodes stored on disks are usually on the order of megabytes to reduce the number of seeks needed.
- Nodes stored in memory should be as small as possible to:
	- Fit into the [[CPU Cache]].
	- Reduce memory fragmentation.
- Nodes size might also depend on the workload.
	- Large sequential scans prefer large nodes, while small nodes are better for point queries.

### Variable-length keys
- Fixed length keys might lead to wasted space. We can solve that using either of the following:
	- Store pointers to the keys instead of storing the keys themselves in the index.
	- Store an index to the key-value pair in a separate dictionary.

### Intra-Node search
- Once we reach a node, we have to find the key inside that node. We can do that using either of the following:
	- Linear Search
	- Binary Search
	- Interpolation Search

## Optimizations

### Pointer Swizzling
- Once a page is brought into the [[Database Buffer Pool]] Manager, we can reference it using a raw pointer instead of its page id.
	- This method skips the overhead of the buffer pool manager latching and searching through its internal structures.
- We must rollback such a change whenever the page is written back to disk.

### Bulk insert
- When initially building a B+ tree, we can build the tree bottom-up to avoid the hustle of repeated split operations.

### Prefix compression
- Sometimes we might have duplicate prefixes of keys within the same node.
- Instead of storing such a prefix normally, we can just store it once and then only include the unique sections of each key.

### Deduplication
- In case of an index over non-unique keys, we might end up with the same key repeated over and over again.
- To solve this, we can store the keys once and then all its associated values after that.


## Latching
- The challenge of B+ tree latching is preventing the following problems:
	- Threads try to modify the contents of a node at the same time.
	- One thread traversing the tree while another thread splits/merges nodes.

### Latch Crabbing/Coupling
- It's a technique to allow multiple threads to access/modify B+ tree at the same time.

#### Mechanism
```
1. Get latch for the parent
2. Get latch for the child
3. Release latch for the parent if the child is deemed "safe".
```
- A "safe" node is one that will not split, merge, or redistribute when updated.
	- The notion depends on the type of operation: insertion or deletion.
- Read latches don't need to worry about the "safe" condition.

#### Protocol
```
- Search:
	1. Start at the rott and go down.
	2. Repeatedly acquire latch on the child and unlatch the parent.
- Insert/Delete:
	1. Start at the root and go down, obtaining X latches as needed.
	2. Once the child is latched, check if it's safe.
		- If child is safe, release latches on all its anecstors.
```
- It's better to release latches that are higher up in the tree since the block access to a larger portion of leaf nodes.

#### Improved Protocol
- The problem with [[#Protocol]] is transactions **always** acquire exclusive latch on the root for every insert/deletion operations, which limits parallelism.
- We can make use of the assumption that split/merge operations are rare and implement the following protocol that optimizes for the common case.
```
- Search: Same algorithm as above.
- Insert/Delete:
	1. Set READ latches as if for search.
	2. Go to leaf.
	3. Set WRITE latche on leaf.
		- If leaf is not safe, release all previous latches and restart the transaction using previous Insert/Delete protocol.
```

### Leaf Node Scans
- Leaf node scans are susceptible to deadlocks because there might be threads trying to acquire X locks in two different directions at the same time.
- To solve this problem we can implement a "no-wait" protocol:
	- If a thread tries to acquire a latch but failed, it should abort its operation and release any latches it holds.

# Sources
- CMU 15-445 Lecture 8 - "Tree Indexes"
- CMU 15-445 Lecture 9 - "Index Concurrency Control"