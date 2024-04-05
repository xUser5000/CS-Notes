# Explanation
- Every host and router on the Internet has an [[IP (Internet Protocol)|IP]] address that can be used in the Source address and Destination address fields of IP packets.
- An IP address does not refer to a host, but rather a network interface.
	- If a host is on two networks, it must have two IP addresses.
	- In practice, most hosts are on one network and thus have one IP address.
- Routers have multiple interfaces and thus multiple IP addresses.

## Prefixes
- IP addresses are hierarchical.
	- Each 32-bit address is comprised of a variable-length network portion in the top bits and a host portion in the bottom bits.
	- The network portion has the same value for all hosts on a single network, such as an Ethernet LAN.
- A Network corresponds to a contiguous block of IP address space.
	- This block is called a **prefix**.
- IP addresses are written in dotted decimal notation.
	- Each of the 4 bytes is written in decimal, from $0$ to $255$.
	- **Example**: the 32-bit hexadecimal address `80D00297` is written as `128.208.2.151`.
- Prefixes are written by giving the lowest IP address in the block and the size of the block.
	- The size is determined by the number of bits in the network portion.
	- By convention, it is written after the prefix IP address as a slash followed by the length in bits of the network portion.
	- **Example**: if the prefix contains $2^8$ addresses, this leaves $24$ bits for the network portion, it is written as $128.208.0.0/24$ .
- Since the prefix length cannot be inferred from the IP address alone, routing protocols must carry the prefixes to routers.
	- The length of the prefix corresponds to a binary mask of 1s in the network portion.
	- When written out this way, it is called a subnet mask and can be ANDed with the IP address to extract only the network portion.
	- **Example**: for $128.208.0.0/24$, the subnet mask is $255.255.255.0$.

### Advantages of hierarchical addresses
- Routers can forward packets based on only the network portion of the address, which makes the routing tables much smaller than they would otherwise be.

### Disadvantages of hierarchical addresses
- Every IP address belongs to a specific network, and routers will only be able to deliver packets destined to that address to the network.
	- Designs such as mobile IP are needed to support hosts that move between networks but want to keep the same IP addresses.
- The hierarchy is wasteful of addresses unless it is carefully managed.
	- If addresses are assigned to networks in (too) large blocks, there will be (many) addresses that are allocated but not in use.
	- It was realized more than two decades ago that the tremendous growth of the Internet was rapidly depleting the free address space.

## Subnets
- // TODO

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.6.2
