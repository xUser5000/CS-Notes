# Explanation
- Extends [[Database Systems/Query Planning & Optimization|Query Planning & Optimization]]
- The query planner receives a query tree from the rewriter and generates a (query) plan tree that can be processed by the executor most effectively.
- The planner in PostgreSQL is based on pure [[Database Systems/Query Planning & Optimization#Cost-based Optimization|cost-based optimization]]. It does not support [[Database Systems/Query Planning & Optimization#Rule-based Optimization|rule-based optimization]] or hints.
- This planner is the most complex subsystem in PostgreSQL.
- As in other RDBMS, the [EXPLAIN](http://www.postgresql.org/docs/current/static/sql-explain.html) command in PostgreSQL displays the plan tree itself. An example is shown below:
```SQL
testdb=# EXPLAIN SELECT * FROM tbl_a WHERE id < 300 ORDER BY data;
                          QUERY PLAN
---------------------------------------------------------------
 Sort  (cost=182.34..183.09 rows=300 width=8)
   Sort Key: data
   ->  Seq Scan on tbl_a  (cost=0.00..170.00 rows=300 width=8)
         Filter: (id < 300)
(4 rows)
```

## Cost Estimation
- Costs are dimensionless values, and they are not absolute performance indicators, but rather indicators to compare the relative performance of operations.
- All operations executed by the executor have corresponding cost functions.
- In PostgreSQL, there are three kinds of costs:
	- The **start-up** cost is the cost expended before the first tuple is fetched. For example, the start-up cost of the index scan node is the cost of reading index pages to access the first tuple in the target table.
	- The **run** cost is the cost of fetching all tuples.
	- The **total** cost is the sum of the costs of both start-up and run costs.
- The [EXPLAIN](https://www.postgresql.org/docs/current/static/sql-explain.html) command shows both of start-up and total costs in each operation.

### Single-table queries

#### Sequential Scan
- In the sequential scan, the start-up cost is equal to 0, and the run cost is defined by the following equation: ![[Sequential Scan Run Cost Formula.png]]
	- Where [seq_page_cost](https://www.postgresql.org/docs/current/static/runtime-config-query.html#GUC-SEQ-PAGE-COST), [cpu_tuple_cost](https://www.postgresql.org/docs/current/static/runtime-config-query.html#GUC-CPU-TUPLE-COST) and [cpu_operator_cost](https://www.postgresql.org/docs/current/static/runtime-config-query.html#GUC-CPU-OPERATOR-COST) are set in the `postgresql.conf` file, and the default values are 1.0, 0.01, and 0.0025, respectively.
	- $N_{tuple}$ and $N_{page}$ are the numbers of all tuples and all pages of this table, respectively.
- PostgreSQL assumes that all pages will be read from storage. In other words, PostgreSQL does not consider whether the scanned page is in the shared buffers or not.

#### Index Scan
- The start-up cost of the index scan is the cost of reading the index pages to access the first tuple in the target table. It is defined by the following equation: ![[Index Scan Start up Cost Formula.png]]
	- where $H_{index}$ is the height of the index tree.
- The run cost of the index scan is the sum of the CPU costs and the I/O (input/output) costs of both the table and the index: ![[Index Scan Run Cost Formula.png]]
	- The first three costs (i.e., index CPU cost, table CPU cost, and index I/O cost) are shown below: ![[Index Scan Run Cost follow-up formulas.png]]
		- [cpu_index_tuple_cost](https://www.postgresql.org/docs/current/static/runtime-config-query.html#GUC-CPU-INDEX-TUPLE-COST) and [random_page_cost](https://www.postgresql.org/docs/current/static/runtime-config-query.html#GUC-RANDOM-PAGE-COST) are set in the `postgresql.conf` file. The defaults are 0.005 and 4.0, respectively.
		- `qual_op_cost` is, roughly speaking, the cost of evaluating the index predicate. The default is 0.0025.
		- [[#Selectivity]] is the proportion of the search range of the index that satisfies the WHERE clause, it is a floating-point number from 0 to 1.
			- ($Selectivity×N_{tuple}$) means the number of the table tuples to be read;  
			- ($Selectivity×N_{index,page}$) means the number of the index pages to be read.
	- ’table IO cost’ is defined by the following equation: ![[Index Scan Run Cost - Table IO Cost.png]]
		- `max_IO_cost` is the worst case of the IO cost, that is, the cost of randomly scanning all table pages.
		- `min_IO_cost` is the best case of the IO cost, that is, the cost of sequentially scanning the selected table page.
		- [[#Index Correlation|indexCorrelation]] is described in below.

##### Selectivity
- The selectivity of query predicates is estimated using either the histogram_bounds or the MCV (Most Common Value), both of which are stored in the statistics information in the [pg_stats](https://www.postgresql.org/docs/current/static/view-pg-stats.html).
- The MCV of each column of a table is stored in the [pg_stats](https://www.postgresql.org/docs/current/static/view-pg-stats.html) view as a pair of columns named `most_common_vals` and `most_common_freqs`:
	- `most_common_vals` is a list of the MCVs in the column.
	- `most_common_freqs` is a list of the frequencies of the MCVs.
- If the MCV cannot be used, e.g., the target column type is integer or double precision, then the value of the _histogram_bounds_ of the target column is used to estimate the cost.
- **histogram_bounds** is a list of values that divide the column’s values into groups of approximately equal population.

##### Index Correlation
- Index correlation is a statistical correlation between the physical row ordering and the logical ordering of the column values (cited from the official document).
- This ranges from -1 to +1.
- Index Correlation is a statistical correlation that reflects the impact of random access caused by the discrepancy between the index ordering and the physical tuple ordering in the table when estimating the index scan cost.

#### Sort
// TODO

### Multiple-Table Queries
- To get the optimal plan tree, the planner has to consider all the combinations of indexes and join methods.
	- This is a very expensive process and it will be infeasible if the number of tables exceeds a certain level due to a combinatorial explosion.
- If the number of tables is smaller than around 12, the planner can get the optimal plan by applying dynamic programming. Otherwise, the planner uses the _genetic algorithm_.

#### Dynamic Programming
- Determination of the optimal plan tree by dynamic programming can be explained by the following steps:
	1. Get the cheapest path for each table.
	2. Get the cheapest path for each combination of two tables.  
		- For example, if there are two tables, A and B, get the cheapest join path of tables A and B. This is the final answer.
		- If there are three tables, get the cheapest path for each of {A, B}, {A, C}, and {B, C}.
	3. Continue the same process until the level that equals the number of tables is reached.
- This way, the cheapest paths of the partial problems are obtained at each level and are used to get the upper level’s calculation. This makes it possible to calculate the cheapest plan tree efficiently.

#### Genetic Query Optimizer
- This is a kind of approximate algorithm to determine a reasonable plan within a reasonable time.
- Hence, in the query optimization stage, if the number of the joining tables is higher than the threshold specified by the parameter [geqo_threshold](http://www.postgresql.org/docs/current/static/runtime-config-query.html#GUC-GEQO-THRESHOLD) (the default is 12), PostgreSQL generates a query plan using the genetic algorithm.

# Sources
- [InterDB - 3. Query Processing](https://www.interdb.jp/pg/pgsql03.html)
