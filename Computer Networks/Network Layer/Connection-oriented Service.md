# Explanation
- If connection-oriented service is used, a path from the source router all the way to the destination router must be established before any data packets can be sent.
	- This connection is called a virtual circuit, in analogy with the physical circuits set up by the telephone system.
	- The network is called a virtual-circuit network.

## Mechanism
- The idea behind virtual circuits is to avoid having to choose a new route for every packet sent.
	-  Each packet carries an identifier telling which virtual circuit it belongs to.
- When a connection is established, a route from the source machine to the destination machine is chosen as part of the connection setup and stored in tables inside the routers.
	- That route is used for all traffic flowing over the connection, exactly the same way that the telephone system works.
- When the connection is released, the virtual circuit is also terminated.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.1.4
