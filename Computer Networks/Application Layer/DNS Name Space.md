# Explanation
- For the Internet, the top of the naming hierarchy is managed by an organization called ICANN (Internet Corporation for Assigned Names and Numbers).
- The Internet is divided into over 250 top-level domains, where each domain covers many hosts.
	- Top-level domains come in two flavors: **generic** and **countries**.
	- Examples: ![[Generic top-level domains.png]]
- Each domain is partitioned into subdomains, and these are further partitioned, and so on.
- All these domains can be represented by a tree.
	- The leaves of the tree represent domains that have no subdomains (but do contain machines, of course).
	- A leaf domain may contain a single host, or it may represent a company and contain thousands of hosts.
	- Illustration: ![[A portion of the Internet domain name space.png]]
- Domain names are case-insensitive.
- Component names can be up to 63 characters long, and full path names must not exceed 255 characters.
- In principle, domains can be inserted into the tree in either generic or country domains.
- Getting a second-level domain, such as `name-of-company.com`, is easy.
	- The top-level domains are run by registrars appointed by ICANN.
	- Getting a name merely requires going to a corresponding registrar (for com in this case) to check if the desired name is available and not somebody else’s trademark.
	- If there are no problems, the requester pays the registrar a small annual fee and gets the name.
- Some of the domains self-organize, while others have restrictions on who can obtain a name.
- Each domain controls how it allocates the domains under it.
	- **Example**: Japan has domains `ac.jp` and `co.jp` that mirror `edu` and `com`.
	- **Example**: The Netherlands does not make this distinction and puts all organizations directly under `nl`.
- To create a new domain, permission is required of the domain in which it will be included.
	- This way, name conflicts are avoided and each domain can keep track of all its subdomains.
- Once a new domain has been created and registered, it can create subdomains, such as `cs.unsd.edu`, without getting permission from anybody higher up the tree.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Chapter 7.1.1
