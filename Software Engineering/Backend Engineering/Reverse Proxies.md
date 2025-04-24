# Explanation
- *Reverse Proxy*: a server that sits in front of one or more web servers, intercepting requests from clients.
	- This is different from a [[Forward Proxies|forward proxy]], where the proxy sits in front of the clients.
- With a reverse proxy, when clients send requests to the origin server of a website, those requests are intercepted at the [network edge](https://www.cloudflare.com/learning/serverless/glossary/what-is-edge-computing/) by the reverse proxy server.
	- The reverse proxy server will then send requests to and receive responses from the origin server.

## Illustration
![[Reverse Proxy Flow.png]]

## Use Cases
- **[[Load balancing]]**
- **Protection from attacks**
	- With a reverse proxy in place, a web site or service never needs to reveal the IP address of their origin server(s).
	- This makes it much harder for attackers to leverage a targeted attack against them, such as a *DDoS attack*. Instead the attackers will only be able to target the reverse proxy which will have tighter security and more resources to fend off a cyber attack.
- **Global server load balancing (GSLB)**: A website can be distributed on several servers around the globe and the reverse proxy will *redirect* clients to the server that’s geographically closest to them. 
- [[Cache|Caching]] content, resulting in faster performance.
- **SSL encryption**
	- Encrypting and decrypting SSL (or TLS) communications for each client can be computationally expensive for an origin server.
	- A reverse proxy can be configured to decrypt all incoming requests and encrypt all outgoing responses, freeing up valuable resources on the origin server.
- **Ingress**: A reverse proxy can act as a router to direct incoming requests to the appropriate microservice based on criteria like the requested API or content.
- **Canary Deployments**:
	- This Allows a small percentage of users to be directed to a new version of an application while the majority of users remain on the old version.
	- Used for testing new features before a full rollout.

## Examples
- 

# Sources
- [Hussein Nasser - Proxy vs Reverse Proxy Server Explained](https://www.youtube.com/watch?v=SqqrOspasag)
- [Cloudflare - What is a reverse proxy? | Proxy servers explained](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)