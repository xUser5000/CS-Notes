# Explanation
- Suppose that there are $n$ replicas, every write must be confirmed by $w$ nodes to be considered successful, and we must query at least $r$ nodes for each read.
- As long as $w + r > n$, we expect to get an up-to-date value when reading, because at least one of the r nodes we’re reading from must be up to date.
	- Reads and writes that obey these $r$ and $w$ values are called *quorum reads and writes*.
	- You can think of $r$ and $w$ as the minimum number of votes required for the read or write to be valid.
- A common choice is to make $n$ an odd number and to set $w = r = (n + 1) / 2$ (rounded up). However, you can vary the numbers as you see fit.
	- For example, a workload with few writes and many reads may benefit from setting $w = n$ and $r = 1$, which makes reads faster, but has the disadvantage that just one failed node causes all database writes to fail.

- The quorum condition, $w + r > n$, allows the system to tolerate unavailable nodes as follows:
	- If $w < n$, we can still process writes if a node is unavailable.
	- If $r < n$, we can still process reads if a node is unavailable.
	- Normally, reads and writes are always sent to all n replicas in parallel. The parameters $w$ and $r$ determine how many nodes we wait for—i.e., how many of the $n$ nodes need to report success before we consider the read or write to be successful.
- If fewer than the required w or r nodes are available, writes or reads return an error.
- Illustration: ![[Quorum Reads and Writes.png]]

## Limitations of Quorum Consistency
- Even with w + r > n, there are likely to be edge cases where stale values are returned:
	- If a write happens concurrently with a read, the write may be reflected on only some of the replicas. In this case, it’s undetermined whether the read returns the old or the new value.
	- If a write succeeded on some replicas but failed on others and overall succeeded on fewer than $w$ replicas, it is not rolled back on the replicas where it succeeded.
		- This means that if a write was reported as failed, subsequent reads may or may not return the value from that write.
	- If a node carrying a new value fails, and its data is restored from a replica carrying an old value, the number of replicas storing the new value may fall below $w$, breaking the quorum condition.

# Sources
- Designing Data-Intensive Applications - Chapter 5
