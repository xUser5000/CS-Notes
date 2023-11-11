# Explanation
- Sorting is potential used in `ORDER BY`, `GROUP BY`, `JOIN`, and `DISTINCT` operators.
	- If the data to be sorted fits in memory, the DBMS uses a standard sorting algorithm (e.g, quick sort).
	- If the data does not fit in memory, then an external sorting method should be used.
	- It should be able to spill to disk as needed and prefer sequential over random IO.
- If a query  contains an `ORDER BY` and a `LIMIT`, the DBMS can use an in-memory priority queue while scanning the data only once.

## External Merge Sort
- It's the standard method used when the data does not fit in memory.
- It's a divide and conquer algorithm that splits the data set into separate *runs* and then sorts them individually.
- It can spill runs to disk as needed then read them back in one at a time.
- It's comprised of two phases:
	1. **Sorting**: sort small chunks of data that fit in memory, then writes the sorted pages back to disk.
	2. **Merge**: combine the sorted sub-files into a larger single file.

### Two-way Merge Sort
- **Mechanism**:
	1. Read each page during the sorting phase, sort it, and write it back to disk.
	2. Read two sorted pages from disk each into its own buffer and merge them together in a third buffer.
	3. Recursively repeat the process until all pages are sorted.
- **Complexity**: $1 + log_2(N)$ passes through the data, where $N$ is the total number of pages.
- **IO Cost**: $2N \cdot P$, where $P$ is the number of passes.
	- since every pass performs an IO read and an IO write for each page.

### General (K-way) Merge Sort
- The generalized version of the algorithm takes advantage of more than three buffer pages.
- Let $B$ be the number of available buffer pages.
	- In the sorting phase, the algorithm can read $B$ pages at a time and write $\frac{N}{B}$ sorted runs back to disk.
	- The merge phase can also combine up to $B - 1$ runs in each pass, using one buffer page for the combined data and writing back to disk as needed.
- **Complexity**: $1 + log_{B-1} (\frac{N}{B})$.
- **IO Cost**: Same as [[#Two-way Merge Sort]]'s IO cost.

### Optimizations

#### Double Buffering Optimization
- While the system is processing a run, we can pre-fetch the next run in the background and store it in a second buffer page.
- This reduces the wait time for IO requests at each step by continuously utilizing the disk.

#### B+ Tree utilization
- The DBMS can use an existing B+ tree index to aid in sorting rather than using the external merge sort algorithm.
- If the index is clustered, the DBMS can just traverse the B+ tree.
	- Since the index is clustered, the data will be stored in the correct order, and the IO access will be sequential.
- If the index is unclustered, traversing the tree is always worse.
	- since record access will require a disk read.

# Sources
- CMU 15-445 Lecture 10 - "Sorting & Aggregation Algorithms"