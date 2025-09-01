# Explanation
- It is widely used in the Internet to configure all sorts of parameters in addition to providing hosts with IP addresses.
- As well as in business and home networks, DHCP is used by ISPs to set the parameters of devices over the Internet access link, so that customers do not need to phone their ISPs to get this information.
- 

## Mechanism
- With DHCP, every network must have a DHCP server that is responsible for configuration.
- The computer broadcasts a request for an IP address on its network.
	- It does this by using a DHCP DISCOVER packet.
- If the server is not directly attached to the network, the router will be configured to receive DHCP broadcasts and relay them to the DHCP server, wherever it is located.
- When the server receives the request, it allocates a free IP address and sends it to the host in a DHCP OFFER packet.
- To be able to do this work even when hosts do not have IP addresses, the server identifies a host using its Ethernet address (which is carried in the DHCP DISCOVER packet).

## How long an IP address should be allocated?
- IP address assignment may be for a fixed period of time, a technique called leasing.
- Before the lease expires, the host must ask for a DHCP renewal. If it fails to make a request or the request is denied, the host may no longer use the IP address it was given earlier.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.6.4
