# Explanation
- Since SQL is a declarative language, the query tells the DBMS what to compute, but now how to compute it.
- The DBMS translates SQL statements into executable query plans.
- The job of the DBMS's query optimizer is to pick an optimal plan to execute a query.
- There are two strategies for query optimization:

## Rule-based Optimization
- In this type of optimization, heuristics match portions of the query plan with known patterns to assemble a plan.
- Rules transform the query to remove inefficiencies.
- Rules never need to examine the data itself.
- Examples of optimization rules include:
	- Perform filters as early as possible (predicate pushdown).
	- Reorder predicates so that the DBMS applies the most selective one first.
	- Breakup a complex predicate and pushing it down (split conjunctive predicates).
	- Perform projections as early as possible to create smaller tuples and reduce intermediate results (projection pushdown).
	- Project out all attributes except the ones requested or requires.
	- Avoid evaluation o predicates whose result does not change per tuple in a table.
	- Merge predicates.
	- Remove unnecessary joins.
	- Flatten nested subqueries.

### Cost-based Optimization
- In this type of optimization, the query optimizer reads the data and estimates the cost of executing equivalent plans.
- The DBMS uses a cost model to estimate the cost of executing a plan.
- After performing rule-based rewriting, the optimizer enumerates different plans for the query and estimate their costs.
	- It then chooses the best plan for the query after exhausting all plans or some timeout.
- The cost of a query depends on:
	- CPU: small cost, but tough to estimate.
	- Disk: the number of block transfers.
	- Memory: amount of DRAM used.
	- Network: the number of messages sent.
- To approximate costs of queries, the DBMS maintains internal statistics about tables, attributes, and indexes in the catalog.
	- These statistics are usually updated in the background.
	- Examples:
		- Number of tuples in a table
		- Number of distinct values of some attribute in a table
- The DBMS can use sampling to apply predicates to a smaller copy of the table with a similar distribution to estimate the selectivity of a predicate.
	- The sample is updated whenever the amount of changes to the underlying table exceeds some threshold.
- For singe-relation queries, the biggest obstacle is choosing the best access method (Sequential scan, binary search, index scan, etc.).

# Sources
- CMU 15-445 Lecture 14 - "Query Planning & Optimization"