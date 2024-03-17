# Explanation
- *Congestion*: a situation where too many packets present in (a part of) the network cause packet delay and loss that degrades performance.
- *Congestion Control*: making sure the network is able to carry the offered traffic.
	- It is a global issue, involving the behavior of all the hosts and routers.
- *Flow Control*: making sure that a fast sender cannot continually transmit data faster than the receiver is able to absorb it.
- The [[Network Layer|network]] and [[Transport Layer|transport]] layer share the responsibility for handling congestion.
- The most effective way to control congestion is to reduce the load that the [[Transport Layer|transport layer]] is placing on the network.

## Scenario
- When the number of packets hosts send into the network is well within its carrying capacity, the number delivered is proportional to the number sent.
- As the offered load approaches the carrying capacity, bursts of traffic occasionally fill up the buffers inside routers and some packets are lost.
	- These lost packets consume some of the capacity, so the number of delivered packets falls below the ideal curve.
- *Congestion Collapse*: a situation in which performance plummets as the offered load increases beyond the capacity.
	- This can happen because packets can be sufficiently delayed inside the network that they are no longer useful when they leave the network.
- *Goodput*: the rate at which useful packets are delivered by the network.

## Illustration
![[Congestion Control.png]]

## Approaches to Congestion Control
- The presence of congestion means that the load is (temporarily) greater than the resources (in a part of the network) can handle.
- The following solutions are usually applied on different time scales to either prevent congestion or react to it once it has occurred:
	- [[Traffic-Aware Routing]]
	- [[Admission Control]]
	- [[Traffic Throttling]]
	- [[Load Shedding]]

### Illustration
![[Timescales of approaches to congestion control.png]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.3.1
