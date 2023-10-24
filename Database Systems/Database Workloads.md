# Explanation
- There are three types of of database workloads:

## OLTP: Online Transaction Processing
- Characterized by fast, short running operations, simple queries that operate on single entity at a time, and repetitive operations.
- Typically handle more writes than reads.
- Example: Amazon storefront.
	- Users can add things to their cart, they can make purchases, but the actions only affect their account.

## OLAP: Online Analytical Processing
- Characterized by long running, complex queries, reads on large portions of the database.
- The DBMS often is analyzing and deriving new data from existing data collected on the OLTP side.

## HTAP: Hybrid Transaction + Analytical Processing
- A combination that tries to do OLTP and OLAP together on the same database.

# Sources
- CMU 15-445 Lecture 5 - "Storage Models & Compression"