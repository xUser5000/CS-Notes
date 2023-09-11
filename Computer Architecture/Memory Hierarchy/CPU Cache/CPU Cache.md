# Explanation
- It's a small, fast SRAM-based (refer to [[Volatile Memory]]) memories managed automatically by the hardware. ![[CPU Cache hierarchy - Intel core i7.png]]
- The programmer does not have control over the CPU cache.
- Organization: ![[CPU Cache Organization.png]]
- Reading:
	1. When the CPU needs to look up data in the main memory, it first checks the cache.
	2. If the desired block resides in the cache memory, the CPU retrieves it from the cache and loads it into registers. (cache hit)
	3. Otherwise, it loads the data from main memory and puts into the cache and the registers.
	4. If we can't put the block in the cache (e.g, because of a capacity/conflict miss), a block is evicted from the cache and is replaced by the new block.
	5. Which block to replace depends on the replacement policy (e.g, random, LRU, etc...).
- Writing is same as reading except we have to make a decision when to write the data to main memory:
	- What to do on a write hit?
		- Write-through: write immediately to main memory
		- Write-back: defer write to main memory until the cache line is replaced.
			- Needs an extra dirty bit (to check if the line is different from main memory or not)
	- What to do on a write miss?
		- Write-allocate: load into cache and update it.
		- No write-allocate: write straight to main memory.

# Sources
- [CMU Introduction to computer systems - Lecture 12](https://scs.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=3395b86e-0bd4-425d-8872-251e714acdd7)