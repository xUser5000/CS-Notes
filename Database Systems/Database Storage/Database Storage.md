# Explanation
- The database is stored as files on disk.
	- Data is organized into fixed-size pages.
		- The first page is the directory page. ^a7626a
			- Maps page IDs to locations in the database files.
		- Pages can contain different kinds of data (tuples, indexes, etc).
		- Each page is given a unique identifier.
		- Most DBMSs have an indirection layer that maps a page id to a file path and offset.
	- Some systems use a file hierarchy, others may use a single file (e.g, SQLite).
		- These files are encoded in a way specific to the DBMS.
- The DBMS's storage manager is responsible for:
	- Representing files as a collection of pages.
	- Keeping track of which pages has bean read and which has been written to.
	- Keeping track of how much free space is there.

## Database Heap
- *Database Heap*: a way to find location of a page given its `page_id`.
- *Heap File*: unordered collection of pages where tuples are stored in random order.
- The DBMS can locate pages using one of the following ways:
	- *Linked List*: Header page holds pointers to a list of free pages and a list of data pages.
	- *Page Directory*: DBMS maintains special pages that track locations of data pages along with the amount of free space on each page.

### Page Layout
- Every page includes a header that records meta-data about the page’s contents:
	- Page size.
	- Checksum.
	- DBMS version.
	- Transaction visibility.
- There are two main approaches to laying out data in pages:
	- [[Slotted Pages]]
	- [[Log-Structured Pages]]

## Tuple Layout
- *Tuple*: sequence of bytes which the DBMS interprets into attribute types and values.
- A tuple contains the following:
	- Tuple Header: contains meta-data about the tuple such as
		- Visibility information for the DBMS’s concurrency control protocol (i.e., information about which transaction created/modified that tuple).
		- Bit Map for NULL values.
	- Tuple Data: contains the actual data for attributes.
		- Attributes are typically stored in the order that you specify them when you create the table.
		- Most DBMSs do not allow a tuple to exceed the size of a page.
	- Unique Identifier
		- Each tuple in the database is assigned a unique identifier.
		- Most common: page id + (offset or slot).
		- An application cannot rely on these ids to mean anything.
- If two tables are related, the DBMS can “pre-join” them, so the tables end up on the same page.
	- **Advantage**: makes reads faster since the DBMS only has to load in one page rather than two separate pages.
	- **Disadvantages**: makes updates more expensive since the DBMS needs more space for each tuple.

## Database Catalog
- In order for the DBMS to be able to decipher the contents of tuples, it maintains an internal catalog to tell it meta-data about the databases.
	- The meta-data contains information about what tables and columns the databases have along with their types and the orderings of the values.
	- Most DBMSs store their catalog inside of themselves in the format that they use for their tables.
		- They use special code to “bootstrap” these catalog tables.

# Topics
- [[Database Workloads]]
- [[Database Storage Models]]
- [[Database Buffer Pool]]

# Sources
- CMU 15-445 Lecture 3 - "Database Storage (Part I)"
- CMU 15-445 Lecture 5 - "Storage Models & Compression"