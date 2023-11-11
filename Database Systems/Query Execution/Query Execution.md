# Explanation

## Query Plan
- The DBMS converts a SQL statement into a query plan.
- Operators in the query plan are arranged in a tree.
	- Typically, operators are binary (i.e, have 1-2 children)
- Data flows from the leaves of this tree towards the root.
- The output of the root node in the tree in the result of the query.
- The same query plan can be executed in multiple ways.

## Processing Models
- It defines how the DBMS executes a query plan.
	- specifies things like direction in which the query plan is evaluated and what kind of data is passed between operators.
- Models can be implemented to invoke operators **top-down** or **bottom-up**.
- Consider the following processing models:

### Iterator Model
#### Description
- The most common processing model that is used by almost every DBMS.
- Works by implementing a `next()` function for every operator.
	- Each node in the query plan call `next()` on its children until the leaf nodes are reached, which start emitting tuples up to their parent nodes.
	- On each call to `next()`, the operator either returns a single tuple or a null marker if there are no more tuples to emit.
- *Pipeline Breakers*: operators that block until children emit all of their tuples.
	- e.g, joins, subqueries, ordering.
#### Illustration
![[Illustration of the Iterator Model.png]]
#### Advantages
- Output control (e.g, LIMIT) works easily with this approach because an operator can stop invoking `Next()` on its child once it has all the tuples it requires.
- Useful in disk-based systems since it allows us to fully use each tuple in memory before the next tuple or page is accessed.
#### Disadvantages
...

### Materialization Model
#### Description
- It's a specialization of the [[#Iterator Model]] where each operator processes its input all at once and then emits its output all at once and emits it as a single result.
	- The output can be a whole tuple (in [[Database Storage Models#N-Ary Storage Model (NSM)|NSM]]) or a subset of columns (in [[Database Storage Models#Decomposition Storage Model (DSM)|DSM]]).
- To avoid scanning too many tuples, the DBMS can propagate down information about how many tuples are needed to subsequent operators.
#### Illustration
![[Illustration of Materialization.png]]
#### Advantages
- Better for [[Database Workloads#OLTP Online Transaction Processing|OLTP]] workloads because queries typically access a small number of tuples at a time.
#### Disadvantages
- Not suited for [[Database Workloads#OLAP Online Analytical Processing|OLAP]] queries with large intermediate results because the DBMS may have to spill those results to disk between operators.

### Vectorization Model
#### Description
- Extends [[#Iterator Model#Description]]
- Each operator emits a batch (i.e vector) of data instead of a single tuple.
- The operator's internal loop implementation is optimized for processing batches of data instead of a single item at a time.
	- It allows operators to use vectorized instructions (SIMD).
#### Illustration
![[Illustration of Vectorization Model.png]]
#### Advantages
- Ideal for OLAP queries that have to scan a large number of tuples because there are fewer invocations of the `Next()` function.

## Access Methods
- *Access Method*: the method the DBMS uses to access the data stored in a table.
- There are two known access methods:

### Sequential Scan
#### Description
- Iterates over every page in the table and retrieves it from the buffer pool.
- It's almost always the least efficient method to execute a query.
- The DBMS maintains an internal cursor that tracks the last page/slot that is examined.
#### Optimizations
- **Pre-fetching**: Fetch next few pages in advance so that the DBMS does not have to block on storage I/O when accessing each page.
- **Buffer Pool Bypass**: The scan operator stores pages that it fetches from disk in its local memory instead of the buffer pool to avoid sequential flooding.
- **Parallelization**: Execute the scan using multiple threads/processes in parallel.
- **Late Materialization**: DSM DBMSs can delay stitching together tuples until the upper parts of the query plan.
	- Allows each operator to pass the minimal amount of information needed to the next operator (e.g. record ID, offset to record in column).
	- This is only useful in column-store systems.
- **Heap Clustering**: Tuples are stored in the heap pages using an order specified by a clustering index.
- **Approximate Queries** (Lossy Data Skipping): Execute queries on a sampled subset of the entire table to produce approximate results.
	- Typically done for computing aggregations in a scenario that allow a low error to produce a nearly accurate answer.
- **Zone Map** (Lossless Data Skipping): Pre-compute aggregations for each tuple attribute in a page.
	- The DBMS can then decide whether it needs to access a page by checking its Zone Map first.

### Index Scan
#### Description
- In an *Index Scan*, the DBMS picks an index to find the tuples that a query needs.
- The DBMS selects an index depending on:
	- What attributes the index contains.
	- What attributes the query references.
	- The attributes's value domains.
	- Predicate composition.
	- Whether the index has unique or non-unique keys.
#### Optimizations
- **Multi-index scans**:
	- When using multiple indexes for a query,
		- the DBMS computes sets of record IDs using each matching index,
		- combines these sets based on the query's predicates,
		- and retrieves the records and apply any predicates that may remain.
	- The DBMS can use bitmaps, hash table, or bloom filters to compute record IDs through set intersection.


## Modification Queries
- Operators that modify the database are responsible for checking constraints and updating indexes.
- For UPDATE/DELETE, child pass record IDs for target tuples and must keep track of previously seen tuples.
- For INSERT, there are two implementation choices:
	- Materialize tuples inside of the operator.
	- Operator inserts any tuple passed in from child operators.

### Halloween Problem
- It's an anomaly in which an update changes the physical location of a tuple, causing a scan operator to visit the tuple multiple times.
- This can occur on clustered tables or index scans.
- The solution is to keep track of the modified record IDs for each query.
- **Bonus**: It was originally discovered by IBM researchers while building System R on Halloween Day in 1976.

## Expression Evaluation
- The DBMS represents a WHERE clause as an expression tree.
	- Illustration: ![[WHERE Clause Expression Tree.png]]
- To evaluate an expression tree at runtime,
	- the DBMS maintains a context handle that contains metadata for the execution, such as the current tuple, the parameters, and the table schema.
	- The DBMS then walks the tree to evaluate its operators and produce a result.

### Optimizations
- The DBMS can do JIT compilation to the expression tree to evaluate the expression directly.
- Based on an internal cost model, the DBMS would determine whether code generation will be adapted to accelerate a query.

# Sources
- CMU 15-445 Lecture 12 - "Query Processing 1"