# Explanation
- There is no generally accepted taxonomy into which all computer networks fit, but two dimensions stand out as important:

## Transmission Technology
- **Point-to-point links**: connect individual pairs of machines.
	- To go from the source to the destination on a network made up of point-to-point links, short messages have to first visit one or more intermediate machines.
- **Broadcast links**: the communication channel is shared by all the machines on the network.
	- Messages sent by any machine are received by all the others.
	- An address field within each packet specifies the intended recipient.

## Scale

### Personal Area Networks (PANs)
- PANS let devices communicate over the range of a person.
- **Example**: a wireless network that connects a computer with its peripherals.

### Local Area Networks (LANs)
- LANs are privately owned networks that operate within and nearby a single building like a home, office, or factory.
- They are widely used to connect personal computers and consumer electronics to let them share resources (e.g., printers) and exchange information.

### Metropolitan Area Networks (MANs)
- MANs covers a city.
- **Example**: Cable Television Networks (not satellites).

### Wide Area Networks (WANs)
- WANs span a large geographical area, often a country or continent.

### Internetworks
- A collection of interconnected networks is called an internetwork or internet.
- Rules of thumb to distinguish internetworks:
	- Different organizations have paid to construct different parts of the network and each maintains its part.
	- The underlying technology is different in different parts.
- *Gateway*: a machine that makes a connection between two or more networks and provides the necessary translation, both in terms of hardware and software.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 1.2
