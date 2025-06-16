# Explanation
- *[[Database Systems|Database]] Cluster*: a collection of _databases_ managed by a PostgreSQL server.
	- The term ‘database cluster’ in PostgreSQL does **not** mean ‘a group of database servers’.
	- A PostgreSQL server runs on a single host and manages a single database cluster.

## Physical Structure
- A database cluster is basically a single directory, referred to as **base directory** that contains some subdirectories and many files.
- Illustration: ![[Example of PostgreSQL database cluster.png]]

# Heap File Layout
- Extends [[Database Storage#Database Heap]]
- Extends [[Database Storage#Page Layout]]
- Extends [[Slotted Pages]]

- Inside a data file (heap table, index, free space map, and visibility map), it is divided into **pages** (or **blocks**) of fixed length, which is 8192 bytes (8 KB) by default.
- The pages within each file are numbered sequentially from 0, and these numbers are called **block numbers**. If the file is full, PostgreSQL adds a new empty page to the end of the file to increase the file size.
- A page within a table contains three kinds of data:
	1. **heap tuple(s)** record data itself and are stacked in order from the bottom of the page.
	2. **line pointer(s)**: 4 bytes long and hold a pointer to each heap tuple.
		- Line pointers form a simple array that plays the role of an index to the tuples.
		- Each index is numbered sequentially from 1, and called **offset number**. 
		- When a new tuple is added to the page, a new line pointer is also pushed onto the array to point to the new tuple.
	3. **header data** is allocated in the beginning of the page.
		- It is 24 byte long and contains general information about the page.  

- Illustration: ![[Page layout of a heap table file in postgresql.png]]

# Sources
- [InterDB - 1. DATABASE CLUSTER, DATABASES, AND TABLES](https://www.interdb.jp/pg/pgsql01.htmll)
