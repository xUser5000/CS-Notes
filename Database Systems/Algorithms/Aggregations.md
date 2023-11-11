# Explanation
- *Aggregation Operator*: an operator in the query plan the collapses the values of one or more tuples into a single scalar value.
- There are two approaches for implementing an aggregation:

## Sorting
- The DBMS first sorts the tuples on the `GROUP BY` keys.
	- It can use either an in-memory sorting algorithm or [[Sorting#External Merge Sort]].
- Then it performs a sequential scan over the sorted data to compute the aggregation.

## Hashing
- Hashing can be computationally cheaper than sorting for computing aggregations.
- The DBMS populates a hash table as it scans the table's contents.
- For each record, check whether there is already an entry in the hash table and perform the appropriate modification.
- If the size of the hash table is too large to fit in memory, the the DBMS spills it to disk. There are two phases to accomplish this:
	1. **Partition**: Use a hash function h1 to split tuples into partitions on disk based on target hash key.
		- This will put all tuples that match into the same partition.
		- The DBMS spills partitions to disk via output buffer. 
	1. **Rehash**:
		1. For each partition on disk, read its pages into memory and build an in-memory hash table based on a second hash function h2 (where $h_1 \neq h_2$).
		2. Go through each bucket of this hash table to bring together matching tuples to compute the aggregation. (assuming each partition fits in memory)

# Sources
- CMU 15-445 Lecture 10 - "Sorting & Aggregation Algorithms"