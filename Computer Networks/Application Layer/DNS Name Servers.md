# Explanation

## Structure
- In theory at least, a single name server could contain the entire DNS database and respond to all queries about it. In practice,
	- this server would be so overloaded as to be useless and
	- if it ever went down, the entire internet would be crippled.
- To avoid such problems, the [[DNS Name Space|DNS name space]] is divided into non-overlapping **zones**.
	- Illustration: ![[Part of the DNS name space divided into zones.png]]
	- Each circled zone contains some part of the tree.
	- The zone boundaries are placed within a zone is up to that zone's administrator.
- Each zone is also associated with one or more name servers.
- A zone has:
	- one primary name server, which gets its information from a file on its disk, and
	- one or more secondary name servers, which get their information from the primary name server.
- To improve reliability, some of the name servers can be located outside the zone.

## Mechanism
- When a resolver has a query about a domain name, it passes the query to a local name server.

### Case 1
- If the domain being sought falls under the jurisdiction of the name server (such as `top.cs.vu.nl` falling under `cs.vu.nl`), it returns the authoritative resource records.
	- *Authoritative Record*: a record that comes from the authority that manages the record and is thus always correct.
	- *Cached Records*: records that may be out of date.

### Case 2
- If the domain being sought is remote (such as when `flits.cs.vu.nl` wants to find the [[Internet Protocol (IP)]] address of `robot.cs.washington.edu`) and there is cached info about the domain available locally, the name server begins a **remote query**.
	1. The query is sent to the local name server.
	2. The local name server asks the **root name servers**.
		- The root name servers have information about each top-level domains.
		- There are 13 root DNS servers.
		- Each root server could logically be a single computer, but since the entire internet depends on them, they are powerful and heavily replicated.
		- Most of the servers are present in multiple geographical locations.
		- The IP addresses of all the root name servers are hard-coded in every computer on earth.
	1. The root name server knows the server for the `edu` domain (in which `cs.washington.edu` is located) and returns the IP address for that part of the answer.
	2. The local name server then continues its quest and sends the entire query to the `edu` name server, which returns the name server for `edu.washington`.
	3. .
	4. .
	5. .
	6. Finally, the local name server arrives at the name server authoritative for `cs.washington.edu` and gets back the IP address from it.

## Queries
- DNS resolution involves two kinds of queries:
	- **Recursive**: when the local name server handles the resolution on behalf of the host machine until it has the desired answer.
		- It does not return partial answers.
	- **Iterative**: when the root or the top-level name servers just return partial answers to the local name server.
		- The local name server is responsible for continuing the resolution by issuing further queries.
		- Iterative queries put the burden on the originator.
		- Root name servers cannot handle recursive queries since they are too busy.

## Caching
- All of the answers, including all the partial answers returned, are cached.
- Cached answers are not authoritative.
- Cache entries should not live too long.
	- That's why the `Time to live` field is included in each [[DNS Domain Resource Records|resource record]], it tells remote servers how long to cache records.

## Underlying Protocol
- DNS messages are sent in UDP packets with a simple format for queries, answers, and name servers that can be used to continue the resolution.
- If no response arrives within a short time, the DNS client repeats the query, trying another server for the domain after a small number of retries.

# Sources 
- Andrew S. Tanenbaum's "Computer Networks" - Chapter 7.1.3
