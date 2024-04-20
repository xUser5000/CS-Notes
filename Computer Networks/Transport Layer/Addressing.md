# Explanation
- When an application (e.g., a user) process wishes to set up a connection to a remote application process, it must specify which one to.
- *ports*: the method that is normally used is to define transport addresses to which processes can listen for connection requests. ^7f27b8

# FAQ
- How does the user process on a particular host know that a particular server is attached to port X?

## Solution 1: Stable port addresses
- The server might have been attaching itself to port X for years and gradually all the network users have learned this.
- In this model, services have stable port addresses that are listed in file in well-known places.
- **Example**: the `/etc/services` file on UNIX systems lists which servers are permanently attached to which ports, including the fact that the mail server is found on TCP port 25.

## Solution 2: Port mapper
1. To find the port corresponding to a given service name, such as ‘‘BitTorrent,’’ a user sets up a connection to the **port mapper**, which is a special process that listens to a well-known port.
2. The user then sends a message specifying the service name, and the port mapper sends back the port of that service.
3. Then the user releases the connection with the port mapper and establishes a new one with the desired service.

- In this model, when a new service is created, it must register itself with the port mapper, giving both its service name and its port.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Section 6.1.1
