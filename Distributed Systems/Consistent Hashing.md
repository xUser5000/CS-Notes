# Explanation
- *Consistent Hashing*: a hashing scheme that
	1. assigns every node to a location on some logical ring,
	2. then the hash of every partition key maps to some location on the ring.
	3. The node that is closest to the key in the clockwise direction is responsible for that key.
- When a node is added or removed, keys are only moved between nodes adjacent to the new/removed node.
- A replication factor of $n$ means that each key is replicated at the $n$ closest nodes in the clockwise direction.

- Illustration: ![[Consistent Hashing.png]]

# Sources
- CMU 15-445 Lecture 21 - "Introduction to Distributed Databases"
