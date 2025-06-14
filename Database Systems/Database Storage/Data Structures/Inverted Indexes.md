# Explanation
- A #datastructure used for **keyword search** over text fields.
- Stores a **mapping from terms to record IDs** where those terms appear.
- Each term maps to a **posting list**, which is a list of record IDs (or tuple pointers).
- Originally known as a **concordance** in early literature.

# Illustration
![[Inverted Index.png]]

## Operations

### Insert(term, record_id)
1. Tokenize the target attribute to extract terms.
2. For each term, add `record_id` to the corresponding posting list.
3. Update the dictionary structure to include any new terms.

### Search(term)
1. Look up the term in the dictionary.
2. Return the corresponding posting list (record IDs where the term appears).

## Use Cases
- Full-text search queries: e.g. `content LIKE '%Pavlo%'`.
- Used in search engines, information retrieval systems, and DBMS text indexes.
- Suitable for finding records by **content**, not structure.

## Advantages
- Efficient for text search on large datasets.
- Can support complex queries (e.g. term frequency, phrase match, proximity).
- Dictionary can be optimized with compression and pre-aggregated statistics.

## Disadvantages
- Requires preprocessing and tokenization of text.
- Updating the index incrementally can be costly.
- Not suitable for range queries or structured data access.

## Variations

### Lucene-style (Finite State Transducer)
- Uses a **[[Trie|trie]]-like structure** (FST) to encode the term dictionary.
- Each edge in the structure carries a **weight**, and the total weight gives the offset in storage.
- Built incrementally and supports compression techniques like **delta encoding** and **bit packing**.
- Often used in combination with vectorized scoring and aggregation support.

### PostgreSQL (GIN Index)
- Uses a [[B+ Tree]] to store the term dictionary.
- Posting lists vary:
	- **Small lists** → simple sorted array of record IDs.
	- **Large lists** → secondary B+Tree to store the record IDs.
- Maintains a separate **pending list** for insert buffering to avoid overhead from small updates.

# Sources
- CMU 15-445/645 (Fall 2024) — "Lecture 09: Indexes II"

# Extra resources
- [ ] Introduction to Information Retrieval (Illustrated Edition)
- [x] [Cockroach Labs - What is an inverted index, and why should you care?](https://www.cockroachlabs.com/blog/inverted-indexes/)
- [x] [Wikipedia - Inverted Index](https://en.wikipedia.org/wiki/Inverted_index)
