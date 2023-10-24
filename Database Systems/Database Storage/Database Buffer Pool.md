# Explanation
- *Buffer Pool*: in-memory cache of pages read from disk.
	- **Purpose**: minimize the movement of pages from [[Disks]] to RAM.
	- Organized as an array of fixed size pages.
		- Each entry is called a frame.
- Most DBMS's use direct IO to bypass the OS's cache in order to avoid redundant copies of pages and having to manage different eviction policies.
	- **Postgres** is an example of a database system that uses the OS’s Page Cache.

## Structure
![[Buffer Pool Structure.png]]
- *Page Table*: in-memory hash table that keeps track of pages that are currently in memory.
	- Maps page IDs to frame locations in the buffer pool.
	- Maintains additional meta-data per page. Examples:
		- dirty flag: indicates that this page has been modified.
		- pin/reference counter: number of threads that are currently using the page.
	- Not to be confused with the page directory ([[Database Storage#^a7626a]]).

## Buffer Replacement Policies
- *Replacement Policy*: algorithm used by the DBMS to decide which pages to evict from buffer pool when it needs space.
	- Implementation Goals: improved correctness, accuracy, speed, and meta-data overhead.
### Least Recently Used (LRU)
- Maintains a timestamp of when each page was last accessed.
	- Can be stored in a separate data structure to improve efficiency.
- The DBMS picks the page with the oldest timestamp to evict.
- *LRU-K*: improved version of LRU that tracks the history of the last K references as timestamps and computes the interval between subsequent accesses.
	- This history is used to predict the next time a page is going to be accessed.
### CLOCK
- It's an approximation of LRU without needing a separate timestamp per page.
- Each page is given a reference bit.
	- When accessed, the bit is set to 1.
- When evicting a page, iterate over all pages.
	- If a page’s bit is set to 1, set to zero.
	- Otherwise, evict it.

## Buffer Pool Optimizations
- There are a number of ways to optimize a buffer pool to tailor it to the application’s workload.

### Multiple Buffer Pools
- The DBMS can maintain multiple buffer pools for different purposes (i.e per-database buffer pool, per-page type buffer pool).
	- Each buffer pool can adopt local policies tailored for the data stored inside of it.
### Pre-fetching
- The DBMS can pre-fetch pages based on the query plan. Then, while the first set of pages is being processed, the second can be pre-fetched into the buffer pool.
	- Used by DBMS’s when accessing many pages sequentially.
### Scan Sharing (Synchronized Scans)
- Query cursors can reuse data retrieved from storage or operator computations.
- This allows multiple queries to attach to a single cursor that scans a table.
### Buffer Pool Bypass
- The sequential scan operator will not store fetched pages in the buffer pool to avoid overhead.

# Sources
- CMU 15-445 Lecture 6 - "Buffer Pools"