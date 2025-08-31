# Explanation

## Problem
- All indexes in PostgreSQL are _secondary_ [[Database Systems/Database Storage/Data Structures/Data Structures|indexes]], meaning that each index is stored separately from the table's main data area (which is called the table's _heap_ in PostgreSQL terminology).
- This means that in an ordinary index scan, each row retrieval requires fetching data from both the index and the heap.
- While the index entries that match a given indexable `WHERE` condition are usually close together in the index, the table rows they reference might be anywhere in the heap.
- The heap-access portion of an index scan thus involves a lot of random access into the heap, which can be slow, particularly on traditional rotating media. (As described in [Section 11.5](https://www.postgresql.org/docs/current/indexes-bitmap-scans.html "11.5. Combining Multiple Indexes"), bitmap scans try to alleviate this cost by doing the heap accesses in sorted order, but that only goes so far.)

## Solution
- To solve this performance problem, PostgreSQL supports _index-only scans_, which can answer queries from an index alone without any heap access.
- The basic idea is to return values directly out of each index entry instead of consulting the associated heap entry.
- There are two fundamental restrictions on when this method is physically possible:
	1. **The index type must support index-only scans**.
		- B-tree indexes always do.
		- The underlying requirement is that the index must physically store, or else be able to reconstruct, the original data value for each index entry. 
		- As a counterexample, GIN indexes cannot support index-only scans because each index entry typically holds only part of the original data value.
	2. The query must reference only columns stored in the index.

### Visibility information
- However, there is an additional requirement for any table scan in PostgreSQL: it must verify that each retrieved row be “visible” to the query's [[Multi-versioning Concurrency Control (MVCC)|MVCC]] snapshot.
- Visibility information is not stored in index entries, only in heap entries; so at first glance it would seem that every row retrieval would require a heap access anyway, which the case if the table row has been modified recently.
	- However, for seldom-changing data there is a way around this problem. 
- An index-only scan, after finding a candidate index entry, checks the [[PostgreSQL Visibility Maps|visibility map]] bit for the corresponding heap page.
	- If it's set, the row is known visible and so the data can be returned with no further work.
	- If it's not set, the heap entry must be visited to find out whether it's visible, so no performance advantage is gained over a standard index scan.
- Even in the successful case, this approach trades visibility map accesses for heap accesses; but since the visibility map is four orders of magnitude smaller than the heap it describes, far less physical I/O is needed to access it.
	- In most situations the visibility map remains cached in memory all the time.

# Sources
- [PostgreSQL 17 Documentation - Index-Only Scans and Covering Indexes](https://www.postgresql.org/docs/current/indexes-index-only-scans.html)
