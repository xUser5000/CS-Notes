# Explanation
- *Congestion Avoidance*: telling the senders to throttle back their transmissions and slow down when congestion is imminent.
- Routers must:
	1. determine when congestion is approaching, ideally before it has arrived.
	2. deliver timely feedback to the senders that are causing the congestion.

## Congestion Detection
- Each router can continuously monitor the either of the following resources:
	- Utilization of output links, or
	- buffering of queued packets inside the router (most useful one), or
	- number of packets that are lost due to insufficient buffering.
- The queueing delay inside routers directly captures any congestion experienced by packets.
	- It should be low most of the time, but will jump when there is a bust of traffic that generates a backlog.

## Congestion Resolution
- Congestion is experienced in the network, but relieving congestion requires action on behalf of the senders that are using the network.
- To deliver feedback, the router must identify the appropriate senders and warn them carefully.
- Routers must be careful with their feedback mechanism in order not to send many more packets into  the already-congested network.
- Different feedback schemes/mechanisms can be used:

### Choke Packets
- The most direct way to notify a sender of congestion is to tell it directly.
- This scheme can help prevent congestion yet not throttle any sender unless it causes trouble.

#### Mechanism
- The router selects a congested packet and sends a choke packet back to the source host, giving it the destination found in the packet.
- The original packet may be tagged so that it will not generate any more choke packets farther along the path.
- To avoid increasing load on the network during a time of congestion, the router may only send choke packets at a low rate.
- When the source host gets the choke packet, it is required to reduce the traffic sent to the specified destination.
- In a datagram network, simply picking packets at random when there is congestion is likely to cause choke packets to be sent to fast senders, because they will have the most packets in the queue.
 
### Explicit Congestion Notification
- This is the method used in the internet network layer.

#### Mechanism
- Instead of generating additional packets to warn of congestion, a router can tag any packet it forwards to signal that it is experiencing congestion.
- When the network delivers the packet, the destination can note that there is congestion and inform the sender when it sends a reply packet.
- The sender can then throttle its transmissions.

#### Illustration
![[Explicit congestion notification.png]]

### Hop-by-Hop Backpressure
// TODO

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.3.4
