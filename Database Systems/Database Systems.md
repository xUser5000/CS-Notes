# Explanation
- *Database*: organized collection of inter-related data that models some aspect of the real world.
- *Database Management System*: software that allows applications to store and analyze information in a database.
	- Designed to allow definition, creation, querying, updating, and administration, of databases in accordance with some data model.
- *Data Model*: collection of concepts for describing the data in a database. Examples:
	- [[Relational Model]] (most common),
	- NoSQL Model (key/value, graph),
	- Array/Matrix/Vectors
- *Schema*: description of a particular collection of data based on a data model.
- Databases commonly has two layers:
	- *Logical Layer* describes which entities and attributes the database has.
	- *Physical Layer* describes how entities and attributes are stored.

# Topics
- [[Database Storage]]
- Algorithms:
	- [[Sorting]]
	- [[Aggregations]]
	- [[Joins]]
- [[Query Execution]]
- [[Concurrent Query Execution]]
- [[Database Systems/Query Planning & Optimization]]
- [[Concurrency Control]]
- [[Crash Recovery]]

# FAQ
- Why do we need a database management system? Why not just store data in a flat CSV file? (AKA Strawman system)? Because there are many issues with this approach:
	- Data Integrity
		- How do we ensure that the artist is the same for each album entry?
		- What if somebody overwrites the album year with an invalid string?
		- How do we treat multiple artists on one album?
		- What happens when we delete an artist with an album?
	- Implementation
		- How do we find a particular record?
		- What if we now want to create a new application that uses the same database?
		- What if two threads try to write to the same file at the same time?
	- Durability
		- What if the machine crashes while our program is updating a record?
		- What if we want to replicate the database on multiple machines for high availability?

# Sources
- CMU 15-445 Lecture 1 - "Relational Model & Relational Algebra"