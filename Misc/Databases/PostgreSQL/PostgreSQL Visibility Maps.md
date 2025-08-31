# Explanation
- Each heap relation has a Visibility Map (VM) to keep track of which pages contain only tuples that are known to be visible to all active [[PostgreSQL Transactions]].
- Note that indexes do not have VMs.
- The visibility map stores two bits per heap page.
	- The first bit, if set, indicates that the page is all-visible, or in other words that the page does not contain any tuples that need to be vacuumed.
		- This information can also be used by [_index-only scans_](https://www.postgresql.org/docs/current/indexes-index-only-scans.html "11.9. Index-Only Scans and Covering Indexes") to answer queries using only the index tuple.
	- The second bit, if set, means that all tuples on the page have been frozen.
- The map is conservative in the sense that we make sure that whenever a bit is set, we know the condition is true, but if a bit is not set, it might or might not be true. 
- Visibility map bits are only set by vacuum, but are cleared by any data-modifying operations on a page.

# Sources
- [PostgreSQL 17 Documentation - Visibility Maps](https://www.postgresql.org/docs/current/storage-vm.html)
