# Explanation
- *Cache*: A smaller, faster storage device that acts as a staging area for a subset of the data in a larger, slower device.
- *Working set*: the set of active cache blocks.
- Cache hit: when a needed block is served from the cache.
- Cache miss: when a needed block is served from the slower storage device.
	- Types of cache misses:
		- Cold miss: occurs because the cache is empty.
		- Conflict miss: occurs because there is some block in the cache that occupied the supposed location of block b.
		- Capacity miss: occurs because the working set is larger than the cache.

# Sources
- Extends [[Memory Hierarchy]]