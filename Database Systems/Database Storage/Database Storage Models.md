# Explanation

## Types
### N-Ary Storage Model (NSM)
- **Description**:
	- The DBMS stores all of the attributes for a single tuple contiguously in a single page.
		- ideal for [[Database Workloads#OLTP Online Transaction Processing]] because it only takes one fetch to get all of the attributes for a single tuple.
- **Advantages**:
	- Fast inserts, updates, and deletes.
	- Good for queries that need the entire tuple.
- **Disadvantages**:
	- Not good for scanning large portions of the table and/or a subset of the attributes.

## Decomposition Storage Model (DSM)
- **Description**:
	- The DBMS stores a single attribute (column) for all tuples contiguously in a block of data. (AKA a “column store.”)
		- ideal for [[Database Workloads#OLAP Online Analytical Processing]] with many read-only queries that perform large scans over a subset of the table’s attributes.
	- To put the tuples back together when using a column store, there are two common approaches:
		- *Fixed-length offsets*:
			- If a value in a given column is at the same offset as another value in another column, this means they are in same tuple.
			- Every single value within the column have to be the same length.
			- Most commonly used approach.
		- *Embedded tuple ids*:
			- For every attribute in the columns, the DBMS stores a tuple id (ex: a primary key) with it.
			- The DBMS also stores a mapping to tell how to jump to every attribute that has that id.
			- Has large storage overhead because it needs to store a tuple id for every attribute entry.
- **Advantages**:
	- Reduces the amount of I/O wasted because the DBMS only reads the data that it needs for that query.
	- Better query processing and data compression
- **Disadvantages**:
	- Slow for point queries, inserts, updates, and deletes because of tuple splitting/stitching.

# Sources
- CMU 15-445 Lecture 5 - "Storage Models & Compression"