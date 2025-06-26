# Explanation
- A #datastructure that uses the **digital representation of keys** (e.g., characters or bits) to index and store data.
- Also known as a **prefix tree** or **digital search tree**.
- The structure is shaped by the key space, not by insertion order.
- Commonly used for **exact-match** and **prefix-based lookups**.

## Illustration
![[Trie.png]]

## Operations

### Insert(x)
1. Represent the key `x` as a sequence of digits (characters or bits).
2. Starting at the root, follow or create a path where each edge corresponds to a digit of the key.
3. Store the value at the final node.

### Find(x)
1. Follow the path determined by the digits of `x`.
2. If the path exists and reaches a terminal node → return success.
3. If any required digit is missing along the path → return not found.

## Use Cases
- Efficient for **exact-match** and **prefix search** in structured keyspaces.
- Common in text indexing, routing tables, or dictionaries.
- Avoids key comparison overhead by walking individual digits.

## Advantages
- No need for rebalancing.
- Lookup complexity is **O(k)** where `k` is the key length, not data size.
- Supports **prefix compression** and high fan-out depending on digit span.

## Disadvantages
- Shape depends on **key content**, not data distribution.
- Can become tall and sparse with variable-length or high-entropy keys.
- Not cache-efficient unless compressed or node-aligned.

## Variations

### Radix Tree (aka Patricia Tree)
- A **vertically compressed** trie.
- Nodes with a single child are merged with their descendants to save space.
- Reduces the number of nodes and increases cache-friendliness.
- May introduce **false positives** — the final match must be validated against the original key.
- Used in systems where space and cache locality are prioritized.

# Sources
- CMU 15-445/645 (Fall 2024) — "Lecture 09: Indexes II"
