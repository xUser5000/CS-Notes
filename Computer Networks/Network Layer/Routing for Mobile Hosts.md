# Explanation
- Mobile hosts include truly mobile situations with wireless devices in moving cars and laptop computers that are used in a series of different locations.
- All mobile hosts are assumed to have a permanent home location that never changes.
- The routing goal in systems with mobile hosts is to make it possible to send packets to mobile hosts using their fixed home addresses and have the packets efficiently reach them wherever they may be.

## Solution 1: Recompute routes online

### Mechanism
- Recompute routes as the mobile host moves and the topology changes.

### Advantages
- Simple.

### Disadvantages
- With a growing number of mobile hosts, this model would soon lead to the entire network endlessly computing new routes.

## Solution 2: Provide mobility above the network layer
- Another alternative would be to provide mobility above the network layer, which is what typically happens with laptops today.

### Mechanism
- When laptops are moved to new Internet locations, they acquire new network addresses.
- There is no association between the old and new addresses; the network does not know that they belonged to the same laptop.

### Advantages
- Does not require additional effort for routing.

### Disadvantages
- Other hosts cannot send packets to the laptop (for example, for an incoming call), without building a higher layer location service, for example, signing into Skype again after moving.
- Connections cannot be maintained while the host is moving; new connections must be started up instead.

## Solution 3: Home agents
- The basic idea used for mobile routing in the Internet and cellular networks is for the mobile host to tell a home agent where it is now.
- *Home Agent*: a host in the home location which acts on behalf of the mobile host.
- Once the home agent knows where the mobile host is currently located, it can forward packets so that they are delivered.
- The mobile host must acquire a local network address before it can use the network.
	- The local address is called a **care of address**.
- If connectivity is lost for any reason as the mobile moves, the home address can always be used to reach the mobile.

### Mechanism
1. Once the mobile host has this address, it can tell its home agent where it is now.
	- It does this by sending a registration message to the home agent with the care of address.
2. The sender sends a data packet to the mobile host using its permanent address.
	- This packet is routed by the network to the host’s home location because that is where the home address belongs.
3. The home agent intercepts the data packet because the mobile host is away from home and then wraps the packet with a new header and sends this bundle to the care of address.
	- This mechanism is called **tunneling**.
4. When the encapsulated packet arrives at the care of address, the mobile host unwraps it and retrieves the packet from the sender.
5. The mobile host sends its reply packet directly to the sender.
6. The sender may learn the current care of address.
	- Subsequent packets can be routed directly to the mobile host by tunneling them to the care of address, bypassing the home location entirely.

### Illustration
![[Packet routing for mobile hosts.png]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.2.10
