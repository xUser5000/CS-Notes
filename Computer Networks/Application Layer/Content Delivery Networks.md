# Explanation
- *CDNs (Content Delivery Networks)* turn the idea of traditional Web caching on its head.
	- Instead of having clients look for a copy of the requested page in a nearby cache, it is the provider who places a copy of the page in a set of nodes at different locations and directs the client to use a nearby node as the server.

## Illustration
![[CDN distribution tree.png]]

## Description
- Using a tree structure has three advantages:
	1. The content distribution can be scaled up to as many clients as needed by using more nodes in the CDN.
		- No matter how many clients there are, the tree structure is efficient.
	2. Each client gets good performance by fetching pages from a nearby server instead of a distant server.
		- This is because the round-trip time for setting up a connection is shorter, TCP slow-start ramps up more quickly because of the shorter round-trip time, and the shorter network path is less likely to pass through regions of congestion in the Internet.
	3. The total load that is placed on the network is also kept at a minimum.
		- If the CDN nodes are well placed, the traffic for a given page should pass over each part of the network only once.
- Most companies do not build their own CDN, they use the services of a CDN provider such as Akamai to actually deliver their content.

## How to organize the clients to use the CDN tree
- The best approach is to use **DNS Redirection**.

- Suppose that a client wants to fetch a page with the URL http://www.cdn.com/page.html.
- To fetch a page, the browser will use DNS to resolve www.cdn.com to an IP address.
	- This DNS lookup proceeds in the usual manner.
	- By using the DNS protocol, the browser learns the IP address of the name server for cdn.com, then contacts the name server to ask it to resolve www.cdn.com.
- The name server is run by the CDN and Instead of returning the same IP address for each request, it will look at the IP address of the client making the request and return different answers.
- The answer will be the IP address of the CDN node that is nearest the client.
- This strategy is perfectly legal according to the semantics of DNS because name servers may return changing lists of IP addresses.

- Illustration: ![[Directing clients to nearby CDN nodes using DNS.png]]

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Chapter 7.5.3
