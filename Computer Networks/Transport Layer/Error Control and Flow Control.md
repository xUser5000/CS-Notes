# Explanation

## Checksum
- The [[Data Link Layer|data link layer]] checksum protects a frame while it crosses a single link.
	- Not essential but valuable for improving performance (since without them a corrupted packet can be sent along the entire path unnecessarily).
- The transport layer checksum protects a segment while it crosses an entire network path.
	- Essential since packets can be corrupted inside a router but not over links.

## Buffer Allocation
- As connections are opened and closed and as the traffic pattern changes, the sender and receiver need to dynamically adjust their buffer allocations.
- The transport protocol should allow a sending host to request buffer space at the other end.
- Buffers could be allocated per connection, or collectively, for all the connections running between the two hosts.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.2.4
