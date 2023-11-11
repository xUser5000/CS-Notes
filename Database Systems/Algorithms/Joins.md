# Explanation
- *Equijoin Join*: a type of join that combines tables where keys are equal.
- For the following algorithms, assume the following:
	- Table R (outer table) contains m tuples in M pages.
	- Table S (inner table) contains n tuples in N pages.
	- The cost is equal to the number of disk IO operations used to compute the join.
		- Output disk IO is ignored.
- Types of join algorithms:

## Nested Loop Join
- Comprised of two nested for loops that iterate over the tuples in both tables and compares each of them pairwise.
	- If the tuples match the join predicate, then output them.
- #outer_table: the table in the outer for loop.
	- Opposite to #inner_table.
### Simple Nested Loop Join
- **Description**:
	- For each tuple in the outer table, compare it with each tuple in the inner table.
- **Cost**: $M + (m \cdot N)$

### Block Nested Loop Join
- **Description**:
	- For each block in the outer table, fetch each block in the inner table and compare all tuples in those two blocks.
- **Cost**: $M + (M \cdot N)$

### Index Nested Loop Join
- **Description**:
	- The DBMS can either:
		- build a temporary index, or
		- use an existing index on one of the tables.
	- The inner table should be the one with the index.
- **Cost**: $M + (m \cdot C)$, where $C$ is the cost of index lookup.

## Sort-Merge Join
- **Mechanism**:
	1. Sort the two tables on ther join keys. ([[Sorting#External Merge Sort]] can be used for this)
	2. Step through each of the tables with cursors and emit matches (just like in merge sort).
- **Description**:
	- Useful when both of the tables are already sorted on join keys (like with a clustered index) or if the output needs to be sorted on the join key anyway.
- **Cost**: Sort + Merge
	- **Merge Cost**: $M + N$

## Hash Join
- The idea is to use a hash table to split up the tuples into smaller chunks based on their join attributes.
- Can only be used in the case of equijoin.

### Basic Hash Join
- **Mechanism**:
	1. **Build**: Scan the outer table and populate a hash table using the hash function h1 on the join attributes.
	2. **Proble**: Scan the inner table and use the hash function h1 on the tuple's join attributes to jump to the corresponding location in the hash table and find a matching tuple.
		- Since there maybe collisions in the hash table, the DBMS will need to examine the original values of the join attributes to determine whether tuples are truly matching.
- **Optimizations**:
	- **Static Hash Table**: if the DBMS knows the size of the outer table, join can use a static hash table.
	- #bloom_filter:
		- **Definition**: A probabilistic data structure that can can fit in the [[CPU Cache]] and answer the question *is key x in the hash table?* with either *definitely no* or *probably yes*.
		- Can reduce disk IO by preventing disk reads that do no result in a n emitted tuple.

### Grace Hash Join
// TODO

# Comparison
![[Comparison Between DBMS Join Algorithms.png]]

# Sources
- CMU 15-445 Lecture 11 - "Joins Algorithms"