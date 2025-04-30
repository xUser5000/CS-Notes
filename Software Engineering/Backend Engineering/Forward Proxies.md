# Explanation
- *Forward Proxy*: a server that sits in front of a group of client machines. When those computers make requests to sites and services on the Internet, the proxy server intercepts those requests and then communicates with web servers on behalf of those clients, like a middleman.
	- Sometimes called a proxy, proxy server, or web proxy.
- 

## Illustration
![[Forward Proxy Flow.png]]

## Use Cases
- Get around state or institutional browsing restrictions by letting users connect to a proxy rather than directly to the sites they are visiting.
- Block a group of users from accessing certain sites.
	- **Example**: a school network might be configured to connect to the web through a proxy which enables content filtering rules, refusing to forward responses from Facebook and other social media sites.
- Protect a user's identity online.
- [[Cache]] frequently accessed content.
	- If another client requests the same content, the proxy can serve it directly from its cache, reducing the need to retrieve it again from the origin server.
- Enable logging of client requests, useful for monitoring website usage or identifying suspicious activity.

# Sources
- [Hussein Nasser - Proxy vs Reverse Proxy Server Explained](https://www.youtube.com/watch?v=SqqrOspasag)
- [Cloudflare - What is a reverse proxy? | Proxy servers explained](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)