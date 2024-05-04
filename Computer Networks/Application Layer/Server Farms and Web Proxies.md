# Explanation
- To build large Web sites that perform well, we can speed up processing on either the server side or the client side:
	- On the server side, more powerful Web servers can be built with a server farm, in which a cluster of computers acts as a single server.
	- On the client side, better performance can be achieved with better caching techniques.
- Neither techniques are sufficient to build the largest websites today, instead [[Content Delivery Networks|content delivery networks]] are used.

## Server Farms
- No matter how much bandwidth one machine has, it can only serve so many Web requests before the load is too great.
- The solution in this case is to use more than one computer to make a Web server.

### Illustration
![[A server farm.png]]

### Description
- The set of computers that make up the server farm must look like a single logical website to clients. There are many solutions to achieve that:
	1. Use [[DNS (Domain Name System)|DNS]] to spread the requests across the servers.
		- When a DNS request is made for the Web URL, the DNS server returns a rotating list of the IP addresses of the servers.
		- Each client tries one IP address, typically the first on the list.
		- The effect is that different clients contact different servers to access the same website.
	2. Make a front-end that sprays incoming requests over the pool of servers.
		- This happens even when the client contacts the server farm using a single destination IP address.
		- The front end is usually a link-layer switch or an IP router, that is, a device that handles frames or packets.
		- All of the solutions are based on it (or the servers) peeking at the network, transport, or application layer headers and using them in nonstandard ways.

## Web Proxies
// TODO

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Chapter 7.5.2
