# Explanation
- Every domain, whether it is a single host or a top-level domain, can have a set of resource records associated with it.
	- These records are the DNS database.
- For a single host, the most common resource record is just its IP address, but many other kinds of resource records also exist.
- When a resolver gives a domain name to DNS, what it gets back are the resource records associated with that name.
- Thus, the primary function of DNS is to map domain names onto resource records.
- Many records exist for each domain and each copy of the database holds information about multiple domains.
	- The order of the records in the database is not significant.

## Structure
- A resource record is a five-tuple: `(Domain name, Time to live, Class, Type, Value)`.
	- *Domain name*: tells the domain to which this record applies.
		- This field is the primary search key used to satisfy queries.
	- *Time to live*: gives indication of how stable the record is.
		- Information that is highly stable is assigned a large value.
		- Information that is highly volatile is assigned a small value.
	- *Class*: For internet information, it's always `IN`.
	- *Type*: tells what kind of record this is.
		- Examples: ![[The principal DNS resource record types.png]]
		- Some hosts have two or more network interfaces, in which case they will have two or more type A or AAAA resource records.
			- Consequently, DNS can return multiple addresses for a single name.
	- *Value*: can be a number, a domain name, or an ASCII string depending on the record type.

# Sources
- Andrew S. Tanenbaum's "Computer Networks" - Chapter 7.1.2
