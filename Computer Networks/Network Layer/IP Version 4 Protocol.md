# Explanation
- An IPv4 datagram consists of a header part and a body or payload part.

## Header Structure

### Illustration
![[The IPv4 (Internet Protocol) header.png]]

### Description
- The header has a 20-byte fixed part and a variable-length optional part.
- The bits are transmitted from left to right and top to bottom, with the high-order bit of the *Version* field going first.

| Field                                  | Purpose                                                                                                                                            | Extra details                                                                                                                                                                                                                                                                                                                                             |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Version                                | Keeps track of which version of the protocol the datagram belongs to.                                                                              | - By including the version at the start of each datagram, it becomes possible to have a transition between versions over a long period of time.<br>- Version 4 dominates the Internet today.                                                                                                                                                              |
| IHL                                    | Tells how long the header is, in 32-bit words.                                                                                                     | - The minimum value is 5, which applies when no options are present.<br>- The maximum value of this 4-bit field is 15, which limits the header to 60 bytes, and thus the Options field to 40 bytes.                                                                                                                                                       |
| Differentiated services                | // TODO                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                           |
| Type of service                        | // TODO                                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                           |
| Total Length                           | Tells the size of everything in the datagram, both the header and data.                                                                            | - The maximum length is 65,535 bytes.                                                                                                                                                                                                                                                                                                                     |
| Identification                         | Allow the destination host to determine which packet a newly arrived fragment belongs to.                                                          | All the fragments of a packet contain the same Identification value.                                                                                                                                                                                                                                                                                      |
| Unused bit                             |                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                           |
| DF                                     | Stands for "Don't fragment", which is an order to the routers not to fragment the packet.                                                          |                                                                                                                                                                                                                                                                                                                                                           |
| MF                                     | Stands for "More Fragments", which all fragments except the last one have set.                                                                     | - It is needed to know when all fragments of a datagram have arrived.                                                                                                                                                                                                                                                                                     |
| Fragment offset                        | Tells where in the current packet this fragment belongs.                                                                                           | - All fragments except the last one in a datagram must be a multiple of 8 bytes, the elementary fragment unit.<br>- Since 13 bits are provided, there is a maximum of 8192 fragments per datagram.                                                                                                                                                        |
| TtL (Time to live)                     | Used to limit packet lifetimes.                                                                                                                    | - It must be decremented on each hop and is supposed to be decremented multiple times when a packet is queued for a long time in a router.<br>- In practice, it just counts hops.<br>- When it hits zero, the packet is discarded and a warning packet is sent back to the source host.<br>- This feature prevents packets from wandering around forever. |
| Protocol                               | Tells it which transport process to give the packet to.                                                                                            | - [[TCP (Transmission Control Protocol)\|TCP]] is one possibility, but so are [[UDP (User Datagram Protocol)\|UDP]] and some others.                                                                                                                                                                                                                      |
| Checksum                               | Checksums the header fields for protection.                                                                                                        | - Must be recomputed at each hop because at least one field always changes (e.g., the TtL field).                                                                                                                                                                                                                                                         |
| Source address and Destination address | Indicate the IP address of the source and destination network interfaces.                                                                          |                                                                                                                                                                                                                                                                                                                                                           |
| Options                                | Was designed to provide an escape to allow subsequent versions of the protocol to include information not present in the original design.          |                                                                                                                                                                                                                                                                                                                                                           |
| Security                               | Tells how secret the information is.                                                                                                               | In practice, all routers ignore it.                                                                                                                                                                                                                                                                                                                       |
| Strict source routing                  | Gives the complete path from source to destination as a sequence of IP addresses.                                                                  | - The datagram is required to follow that exact route.<br>- Useful to send emergency packets when the routing tables have been corrupted, or for making timing measurements.                                                                                                                                                                              |
| Loose source routing                   | Requires the packet to traverse the list of routers specified, in the order specified, but it is allowed to pass through other routers on the way. |                                                                                                                                                                                                                                                                                                                                                           |
| Record route                           | Tells each router along the path to append its IP address to the Options field.                                                                    | This allows system managers to track down bugs in the routing algorithms.                                                                                                                                                                                                                                                                                 |
| Timestamp                              | like the *Record route* option, except that in addition to recording its 32-bit IP address, each router also records a 32-bit timestamp.           |                                                                                                                                                                                                                                                                                                                                                           |

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 5.6.1