# Explanation
- The relational model defines a database abstraction based on relations to avoid maintenance overhead.
	- It has three key points:
		- Store databases in simple data structures (called relations)
		- Access data through high-level language, DBMS figures out best execution strategy.
		- Physical storage left up to the DBMS implementation.
	- It has three concepts:
		- Structure: The definition of relations and their contents.
			- i.e, the attributes the relations have and the values that those attributes can hold.
		- Integrity: Ensure the database’s contents satisfy constraints.
			- An example constraint would be that any value for the year attribute has to be a number.
		- Manipulation: How to access and modify a database’s contents.
- *Relation*: unordered set that contains the relationship of attributes that represent entities.
	- Since the relationships are unordered, the DBMS can store them in any way it wants, allowing for optimizations.
- *Tuple* set of attribute values in the relation.
	- Values can be lists or nested data structures.
	- Every attribute can be a special value, NULL, which means for a given tuple the attribute is undefined.
- *Primary Key*: attribute that uniquely identifies a single tuple.
	- Some DBMSs automatically create an internal primary key if you don't define one.
	- Most DBMSs have support for auto-generated primary keys so an application does not have to manually increment the keys, but a primary key is still required for some DBMSs.
- *Foreign Key*: specifies that an attribute from one relation has to map to a tuple in another relation.
- *Data Manipulation Language (DML)*: describes methods to store and retrieve information from a database. There are two classes for such languages:
	- *Procedural*: The query specifies the (high-level) strategy the DBMS should use to find the desired result based on sets / bags. (relational algebra)
	- Non-Procedural (Declarative): The query specifies only what data is wanted and not how to find it. (relational calculus)
- *Relational Algebra*: set of fundamental operations to retrieve and manipulate tuples in a relation.
	- Each operator takes in one or more relations as inputs, and outputs a new relation.
	- To write queries we can “chain” these operators together to create more complex operations.
	- Relational algebra is a procedural language because it defines the high level-steps of how to compute a query.
	- Examples of operations: Select, Projection, Union, Intersection, Join, etc.

# Sources
- Extends [[Database Systems#Sources]]