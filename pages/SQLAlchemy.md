## Components
	- SQLAlchemy ORM
	- SQLAlchemy Core
		- Schema/types
		- SQL Expression Language
			- provides a system of constructing SQL expressions represented by
			  composable objects, which can then be “executed” against a target database
			  within the **scope of a specific transaction**, returning a result set
			- Inserts, updates and deletes (i.e. [DML]) are achieved by passing
			  SQL expression objects representing these statements along with **dictionaries**
			  that represent **parameters** to be used with each statement.
		- Engine
		- Connection pooling
		- Dialect
	- DBAPI
- ## Concepts
	- unit of work
		- translate **changes in state** against **mutable objects** into INSERT, UPDATE and DELETE **constructs** which are then invoked in terms of those objects
-
- ## Learnt
	- can use Core directly to query DB
	- Core/SQL Expression language is command oriented whereas the ORM is state oriented.
	- DLM tasks refer to the **persistence of business objects** in database