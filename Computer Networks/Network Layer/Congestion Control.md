# Explanation
- *Congestion*: a situation where too many packets present in (a part of) the network cause packet delay and loss that degrades performance.
- *Congestion Control*: making sure the network is able to carry the offered traffic.
	- It is a global issue, involving the behavior of all the hosts and routers.
- *Flow Control*: making sure that a fast sender cannot continually transmit data faster than the receiver is able to absorb it.
- The [[Network Layer|network]] and [[Transport Layer|transport]] layer share the responsibility for handling congestion.

## Approaches to Congestion Control
- The presence of congestion means that the load is (temporarily) greater than the resources (in a part of the network) can handle.
- The following solutions are usually applied on different time scales to either prevent congestion or react to it once it has occurred:
	- [[Traffic-Aware Routing]]

### Illustration
![[Timescales of approaches to congestion control.png]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.3.1
