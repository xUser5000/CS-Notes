# Explanation
- A #datastructure for performing **nearest-neighbor search** over vector embeddings.
- Each record is mapped to a high-dimensional vector (e.g., via an embedding model).
- Queries return vectors that are **geometrically close** to the input vector.
- Used when semantic similarity is more important than exact matching.

## Operations

### Insert(vector, record_id)
1. Assign a vector to the record (e.g., using an embedding function).
2. Add the vector and its record ID to the index.
3. Depending on the structure, update partitioning or graph topology.

### Search(query_vector)
1. Use the index to locate candidate vectors close to `query_vector`.
2. Return nearest neighbors based on vector distance (e.g., cosine, Euclidean).
3. Optionally, apply post-filtering or re-ranking on matched records.

## Use Cases
- Semantic search over documents, images, or audio.
- Similarity matching in recommendation systems.
- Used in systems where **textual matching is insufficient**, and embeddings represent richer meaning.

## Advantages
- Supports **approximate nearest neighbor** search in high-dimensional spaces.
- Enables **semantic** queries, even when exact terms don’t match.
- Can work with many distance metrics (e.g., cosine, L2).

## Disadvantages
- No well-defined “correct” result — depends on vector space quality.
- Accuracy/speed trade-offs require tuning.
- More complex to implement and query than traditional indexes.

## Variations

### IVFFlat (Inverted File)
- Clusters vectors into groups using a clustering algorithm (e.g., k-means).
- During query:
	1. Use same clustering method to map query to a group.
	2. Search vectors within that group (and optionally neighboring groups).
- May quantize vectors to reduce dimensionality and improve performance.

### HNSW (Hierarchical Navigable Small World)
- Builds a **multi-layer graph** where each node connects to its nearest neighbors.
- During query:
	1. Start at the top layer and greedily traverse edges to get closer to the query vector.
	2. Drop to lower layers for refinement.
- Used in libraries like FAISS and hnswlib.

# Sources
- CMU 15-445/645 (Fall 2024) — "Lecture 09: Indexes II"
