# Explanation
- DBMS uses various data structures for many different parts.
- Design decisions to consider when implementing data structures:
	- **Data Organization**: how to layout memory and what information to store to support efficient access.
	- **Concurrency**: how to enable multiple threads to access the data structure without causing problems.
- *Table index*: replica of a subset of a table's columns stored for efficient access.
	- e.g, instead of performing a sequential scan, the DBMS can search the table's auxiliary index to find tuples more quickly.
	- The table's contents and the index are always in sync.
	- More indexes makes queries run faster but they use storage and have maintenance overhead.
- *Clustered Index*: index whose order of the rows in the data pages corresponds to the order of the rows in the index. [2]
	- Only one clustered index can exits for any particular table.

# Topics
- [[Hash Table]]
- [[B+ Tree]]
- [[Database Systems/Database Storage/Data Structures/Concurrency|Concurrency]]

# Source
1. CMU 15-445 Lecture 7 - "Hash Tables"
2. [IBM - Clustered vs non-clustered indexes](https://www.ibm.com/docs/en/ias?topic=indexes-clustered-non-clustered)