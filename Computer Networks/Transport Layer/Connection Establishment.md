# Explanation

## Problem 1: Delayed Duplicates
- The crux of the problem is that the delayed duplicates are thought to be new packets.
- We cannot prevent packets from being duplicated and delayed but if and when this happens, the packets must be rejected as duplicates and not processed as fresh packets.

- Packet lifetime can be restricted to a known maximum using one (or more) of the following techniques:
	- **Restricted network design** that prevents packets from looping, combined with some way of bounding delay including congestion over the (now known) longest possible path.
	- **Putting a hop counter in each packet** that is decremented each time the packet is forwarded.
		- The network protocol discards any packet whose counter becomes zero.
	- **Timestamping each packet**
		- Requires the router clocks to be synchronized which is a nontrivial task.
		- In practice, a hop counter is a close approximation to age.
- To guarantee that a packet and all its acknowledgements are dead, we can introduce a period $T$ that is a multiple of the true maximum packet lifetime.
	- i.e., If we wait $T$ secs after a packet has been sent, we can be sure that all traces of it are now gone.

### Solution 1: Throwaway Transport Addresses

#### Mechanism
- Each time a transport address is needed, a new one is generated.
- When a connection is released, the address is discarded and never used again.
- Delayed duplicate packets never find their way to a transport process and can do no damage.

#### Advantages

#### Disadvantages
- This approach makes it more difficult to connect with a process in the first place.

### Solution 2: Sequence Numbers for Connections

#### Mechanism
- Give each connection a unique identifier chosen by the initiating party and put in each segment, including the one requesting the connection.
- After each connection is released, each transport entity can update a table listing obsolete connections as (peer transport entity, connection identifier) pairs.
- Whenever a connection request comes in, it can be checked against the table to see if it belongs to a previously released connection.

#### Advantages

#### Disadvantages
- Requires each transport entity to maintain a certain amount of history information indefinitely, which must persist at both the source and the destination machines.
	- If a machine crashes and loses its memory, it will no longer know which connection identifiers have already been used by it peers.

### Solution 3: Killing aged packets

#### Mechanism
- The heart of the method is for the source to label segments with sequence numbers that will not be reused within $T$ secs.
- The period $T$ and the rate of packets per second determine the size of the sequence numbers.
- In this way, only one packet with a given sequence number may be outstanding at any given time. 

#### Advantages
- Duplicates of this packet may still occur, and they can be easily discarded by the destination.
- It is no longer the case that a delayed duplicate of an old packet may beat a new packet with the same sequence number and be accepted by the destination in its stead.

#### Disadvantages
- 

## Problem 2: Machine Crashes

### Solution 1: Idling for $T$ seconds
- To get around the problem of a machine losing all memory of where it was after a crash, one possibility is to require transport entities to be idle for $T$ secs after a recovery.
- The idle period will let all old segments die off, so the sender can start again with any sequence number.
- In a complex internetwork, $T$ may be large, so this strategy is unattractive.

### Solution 2: Time-of-day clock
- We equip each host with a time-of-day clock, which takes the form of a binary counter that increments itself at uniform intervals.
	- The number of bits in the counter must equal or exceed the number of bits in the sequence numbers.
	- The clock is assumed to continue running even if the host goes down.
	- The clocks at different hosts need not be synchronized.
- When a connection is set up, the low-order $k$ bits of the clock are used as the $k$-bit initial sequence number.
- The sequence space should be so large that by the time sequence numbers wrap around, old segments with the same sequence number are long gone.
- Once the transport entities have agreed on the initial sequence number, any sliding window protocol can be used for data flow control.

## Three-way Handshake
- Since we do not normally remember sequence numbers across connections at the destination, we still have no way of knowing if a CONNECTION REQUEST segment containing an initial sequence number is a duplicate of a recent connection.
	- The three-way handshake solves this problem.
- TCP uses this three-way handshake to establish connections.
	- Within a connection, a timestamp is used to extend the 32-bit sequence number so that it will not wrap within the maximum packet lifetime, even for gigabit-per-second connections.

### Mechanism
1. Host 1 chooses a sequence number, $x$, and sends a **CONNECTION REQUEST** segment containing it to host 2.
2. Host 2 replies with an ACK segment acknowledging $x$ and announcing its own initial sequence number, $y$.
3. Host 1 acknowledges host 2’s choice of an initial sequence number in the first data segment that it sends.

### Illustration
![[Normal operation of the three-way handshake.png]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.2
