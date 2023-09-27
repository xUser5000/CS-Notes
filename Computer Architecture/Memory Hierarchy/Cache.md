# Explanation
- *Cache*: A smaller, faster storage device that acts as a staging area for a subset of the data in a larger, slower device.
- *Working set*: the set of active cache blocks.
- #cache_hit: when a needed block is served from the cache.
- #cache_miss: when a needed block is served from the slower storage device.
	- Types of cache misses:
		- Cold miss: occurs because the cache is empty.
		- Conflict miss: occurs because there is some block in the cache that occupied the supposed location of block b.
		- Capacity miss: occurs because the working set is larger than the cache.

## Write Behavior

### Write-back (Immediate Reporting)
- Acknowledge the write has been completed when the data has been written to the cache.
### Write-through
- Acknowledge the write has been completed when the data has been committed to the next larger device in the [[Memory Hierarchy]].

# Sources
- Extends [[Memory Hierarchy]]