# Explanation
- A #datastructure that maintains a **sorted list of keys** using multiple levels of linked lists.
- Each level skips over some keys to allow faster traversal than a flat list.
- Provides **approximate O(log n)** search time, similar to [[B+ Tree|B+Tree]], but without needing rebalancing.
- Typically used as an **in-memory index structure**, unlike disk-oriented trees.

## Operations

### Find(x)
1. Start at the highest level.
2. Traverse forward until the next key is greater than or equal to `x`.
3. Move down one level and repeat until reaching the bottom.
4. If the key at the final position equals `x`, it is found.

### Insert(x)
1. Insert `x` in the bottom level in sorted order.
2. Flip a coin to decide how many higher levels `x` should be added to.
3. Insert `x` in each of the selected levels.
4. Each level’s insertion is done bottom-up to avoid race conditions with other readers.

### Delete(x)
1. Mark the key as logically deleted at each level.
2. Readers ignore logically deleted nodes.
3. Later, physically remove the key from top to bottom.

## Use Cases
- Used as the indexing structure in **memtables** of [[LSM Tree]]s.
- Suitable for **lock-free concurrent systems** where balancing is a bottleneck.
- Often found in **in-memory stores** and runtime-managed indexes.

## Advantages
- Does not require rebalancing like [[B+ Tree|B+Tree]]s.
- Simple, fast insertions and deletions.
- Memory-efficient when reverse pointers are excluded.
- Naturally supports **lock-free concurrency**.

## Disadvantages
- Not cache- or disk-friendly due to pointer chasing.
- **Reverse range scans** require extra pointers or logic.
- May be less predictable than tree structures for on-disk access.

# Sources
- CMU 15-445/645 (Fall 2024) — "Lecture 09: Indexes II"
