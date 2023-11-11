# Explanation
- *Hash Table*: implementation of an associative array abstract data type that maps keys to values.

## Hash Function
- It describes how to map a large key space into a smaller domain.
- Takes a key as input and returns an integer representation of the key.
	- Used to compute an index into an array of buckets.
- #deterministic: same key always produces the same hash.
- The DBMS need not use cryptographic hash functions (e.g, SHA-256).
	- Because key security is not important.
- The current state of the art hash function is Facebook's XXHash3.
- **trade-off**: fast hash function that has lots of collisions vs slow hash function with few collisions.

## Hashing Scheme
- It describes how to handle key collisions after hashing.
	- **trade-off**: allocating a large hash table to avoid collisions vs having to execute additional instructions when a collisions occurs.

### Static hashing schemes
- Assumes the size of the hash table is fixed.
	- If the table gets filled, a new larger table should be built from scratch.

#### Linear Probe Hashing
- Uses a circular buffer.
- Maps keys to slots in the buffer.
- When a collision occurs, we linearly search the adjacent slots until an open one is found.
- For look-ups, we check the slot the key hashes to and search linearly until we find the desired entry.
	- If we reach the empty slot or the table is filled, then the key is not present.
- Needs to store both the key and the value in the slot.
- For deletions, we can't just delete an entry since this may prevent future look-ups from finding entries that have been put below the not empty slot.
	- To solve this problem, there are two approaches:
		- **Tombstones**: Instead of deleting an entry, we replace it with a tombstone entry which tells future look-ups to keep scanning.
		- **Shifting**: Shift the adjacent data to fill the empty slot.
- In the case of **non-unique** keys:
	- Store a linked list with each key and append to that list whenever a new value of the same key gets inserted.
	- Store the same key multiple times in the table.

#### Robin-hood Hashing
- It's an extension of [[#Linear Probe Hashing]] that tries to reduce the maximum distance of each key from their optimal position.
- Each entry records how far away it is from its optimal position.
- On insert, If the key being inserted would be farther away from its optimal position than the current key's distance, we replace the current entry and continue trying to insert the old entry farther down the table.

#### Cuckoo Hashing
- Maintains multiple hash tables with multiple hash functions.
- On insert, we check every table and choose one that has an free slot.
	- If no table is free, we evict a random entry and repeat the process.
- If a cycle of evictions happen, we build larger tables and re-insert the keys.
- Guarantees $O(1)$ lookups and deletions, but insertion may be more expensive.

### Dynamic Hashing
- Dynamic hashing schemes can resize the hash table on demand without needing to rebuild the entire table.

#### Chained Hashing
- Maintains a linked-list of buckets for every key slot.
- Keys which hash to the same slot are simply inserted into the linked list for that slot.

#### Extendible Hashing
- Improved version of [[#Chained Hashing]] that splits the buckets instead of letting chains to grow forever.
- Allows multiple slot locations to point to the same bucket chain.
- Only moves data within the buckets of the split chain.

#### Linear Hashing
// TODO

## Latching

### Latching Static Tables
- Latching a static hash table is easy because:
	- All threads move in the same direction when moving from a slot to the next.
	- All threads only access one page/slot at a time.
- Deadlocks are not possible due to above reasons.
- When resizing, we can just acquire a global latch on the entire table.

### Latching Dynamic Tables
- There are two possible approaches to latch dynamic hash tables.

#### Page latches
- Each page has its own [[Database Systems/Database Storage/Data Structures/Concurrency#Reader-Writer Latch|Reader-Writer Latch]] that protects is entire contents.
- Decreases parallelism because potentially only one thread can access a page at a time.
- Accessing multiple slots in a page will be faster because a thread has to only acquire a single latch.

#### Slot latches
- Each slot has its own latch.
- Increases parallelism because two threads can access different slots on the same page.
- Increases storage and computational overhead because
	- Threads have to acquire a latch for every slot they access.
	- Each slot has to store data for latches.
- The DBMS can use single mode latches to reduce meta-data and computational overhead at the cost of some parallelism.

# Sources
- CMU 15-445 Lecture 7 - "Hash Tables"
- CMU 15-445 Lecture 9 - "Index Concurrency Control"