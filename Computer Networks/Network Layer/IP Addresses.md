# Explanation
- *ICANN*: a nonprofit corporation called that manages network numbers to avoid conflicts.
- Every host and router on the Internet has an [[Internet Protocol (IP)|IP]] address that can be used in the Source address and Destination address fields of IP packets.
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
- *Subnetting*: allowing the block of addresses to be split into several parts for internal use as multiple networks, while still acting like a single network to the outside world.
- *Subnet*: the network (such as Ethernet LANs) that result from dividing up a larger network.
- Allocating a new subnet does not require contacting ICANN or changing any external databases.

## NAT (Network address translation)
- IP addresses are scarce.
	- **Example**: an ISP might have a /16 address, giving it 65,534 usable host numbers. If it has more customers than that, it has a problem.
- The problem of running out of IP addresses is not a theoretical one that might occur at some point in the distant future. It is happening right here and right now.
- The long-term solution is for the whole Internet to migrate to IPv6, which has 128-bit addresses.

### Description
- NAT is widely used in practice, especially for home and small business networks, as the only expedient technique to deal with the IP address shortage.
	- It's unlikely to go away even when IPv6 is widely deployed.
- The basic idea behind NAT is for the ISP to assign each home or business a single IP address (or at most, a small number of them) for Internet traffic.
- Within the customer network, every computer gets a unique IP address, which is used for routing intramural traffic.
- Just before a packet exits the customer network and goes to the ISP, an address translation from the unique internal IP address to the shared public IP address takes place.
- This translation makes use of three ranges of IP addresses that have been declared as private:
	- 10.0.0.0 - 10.255.255.255/8 (16,777,216 hosts)
	- 172.16.0.0 – 172.31.255.255/12 (1,048,576 hosts)
	- 192.168.0.0 – 192.168.255.255/16 (65,536 hosts)
- Networks may use the private addresses internally as they wish but no packets containing these addresses may appear on the internet.

### Mechanism
- Within the customer premises, every machine has a unique address of the form 10.x.y.z.
- Before a packet leaves the customer premises, it passes through a **NAT box** that converts the internal IP source address to the customer’s true IP address.
- The NAT box is often combined in a single device with a firewall, which provides security by carefully controlling what goes into the customer network and what comes out of it.
- It is also possible to integrate the NAT box into a router or ADSL [[Modem]].

### How does the NAT box translates incoming packets?
- NAT makes use of the fact that most IP packets carry either [[TCP (Transmission Control Protocol)|TCP]] or [[UDP (User Datagram Protocol)|UDP]] payloads, which contains a source and a destination [[Addressing#^7f27b8|ports]].
- When an outgoing packet enters the NAT box,
	- the 10.x.y.z source address is replaced by the customer’s true IP address, and
	- the TCP Source port field is replaced by an index into the NAT box’s 65,536-entry translation table, and
		- This table entry contains the original IP address and the original source port.
	- both the IP and TCP header checksums are recomputed and inserted into the packet.
- When a packet arrives at the NAT box from the ISP,
	- the Source port in the TCP header is extracted and used as an index into the NAT box’s mapping table, and
	- from the entry located, the internal IP address and original TCP Source port are extracted and inserted into the packet.

### Disadvantages of NAT
- NAT violates the architectural model of IP, which states that every IP address uniquely identifies a single machine worldwide.
	- With NAT, thousands of machines may (and do) use address 10.0.0.1.
- NAT breaks the end-to-end connectivity model of the internet, which says that any host can send a packet to any other host at any time.
	- Since the mapping in the NAT box is set up by outgoing packets, incoming packets cannot be accepted until after outgoing ones.
- NAT changes the Internet from a connectionless network to a peculiar kind of connection-oriented network.
	- The NAT box must maintain information (i.e., the mapping) for each connection passing through it.
	- If the NAT box crashes, all its TCP connections are destroyed.
- NAT violates the most fundamental rule of protocol layering: layer k may not make any assumptions about what layer k + 1 has put into the payload field. ([[Network Software#^82572c]]).
- The use of transport protocols other than TCP or UDP will make NAT fail.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.6.2
