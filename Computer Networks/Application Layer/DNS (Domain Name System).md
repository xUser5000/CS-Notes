# Explanation
- Problems:
	- [[Internet Protocol (IP)]] addresses are hard for people to remember.
	- Browsing a company’s Web pages from a particular IP address means that if the company moves the Web server to a different machine with a different IP address, everyone needs to be told the new IP address.
- We need to introduce a high-level, readable names to decouple machine names from machine addresses.

## Solution 1: ARPANET's local file
- Way back in the ARPANET days, there was simply a file, `hosts.txt`, that listed all the computer names and their IP addresses.

### Mechanism
- Every night, all the hosts would fetch it from the site at which it was maintained.

### Problems
- In a large-scale network, the file would be too large.
- Host name conflicts would occur constantly unless names were centrally managed (which is unthinkable in a huge international network due to the load and latency).

## Solution 2: DNS
- The essence of DNS (Domain name system) is the invention of a hierarchical, domain-based naming scheme and a distributed database system for implementing this naming scheme.
- It's used for mapping host names to IP addresses.
- *Name Resolution*: the process of looking up a name and finding an address.

### Mechanism
- To map a name onto an IP address, an application program calls a library procedure called the resolver, passing it the name as a parameter.
- The resolver sends a query containing the name to a local DNS server, which looks up the name and returns a response containing the IP address to the resolver, which then returns it to the caller.
- The query and response messages are sent as [[UDP (User Datagram Protocol)|UDP]] packets.

### Topics
- [[DNS Name Space]]
- [[DNS Domain Resource Records]]
- [[DNS Name Servers]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Chapter 7.1
