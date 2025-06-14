# Skip List

## Explanation
- A #datastructure that maintains a **sorted list of keys** using multiple levels of linked lists.
- Each level acts as a shortcut over the level below, allowing **faster traversal**.
- The bottom level contains **all entries**, while each higher level links a **subset** (e.g., every 2nd, 4th key).
- Avoids the need for global rebalancing, unlike tree-based structures.

## Illustration
![[Skip List.png]]

## Operations

### Find(x)
1. Start at the top-most level.
2. Traverse forward until the next key is greater than or equal to `x`.
3. Move down one level and repeat until reaching the bottom.
4. If key `x` is present at the bottom level, return it.

### Insert(x)
1. Insert the key in sorted order at the bottom level.
2. Flip a coin to determine how many higher levels to include `x` in.
3. For each selected level, insert `x` in sorted order.
4. Each insertion is local — pointer swaps suffice, enabling lock-free memory updates.

### Delete(x)
1. Mark the node as logically deleted at each level (e.g., with a flag).
2. Readers skip over deleted nodes.
3. Later, physically remove the key top-down.

## Use Cases
- Common in **in-memory structures** like LSM tree **memtables**.
- Suitable for concurrent environments where **rebalancing is undesirable**.
- Great for dynamic, write-heavy applications.

## Advantages
- **No rebalancing** required for inserts or deletes.
- **Efficient** average-case performance (~O(log n)).
- Uses **less memory** than B+Trees when reverse pointers are omitted.
- Easy to make **concurrent and lock-free**.

## Disadvantages
- Not cache- or disk-friendly due to pointer chasing.
- Reverse traversal is **non-trivial** without extra pointers.
- Performance less predictable than B+Trees for range queries on disk.

## Sources
- CMU 15-445/645 (Fall 2024) — "Lecture 09: Indexes II"
