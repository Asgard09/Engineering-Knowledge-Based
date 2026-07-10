# 1. Keys
|Key Type|Description|
|---|---|
|**Primary Key (PK)**|Uniquely identifies each row; cannot be NULL|
|**Composite Key**|PK made of two or more columns|
|**Foreign Key (FK)**|References a PK in another table; enforces referential integrity|
|**Candidate Key**|Any column(s) that could serve as PK|
|**Surrogate Key**|Artificial key (e.g. auto-increment ID) with no business meaning|
|**Natural Key**|Key derived from real-world data (e.g. SSN, email)|
|**Unique Key**|Enforces uniqueness but allows NULLs (unlike PK)|

# 2. SQL and commands
SQL stands for Structure Query Language, is the standard language used to communicate with relational database. It allows to create database object, query data, modify data, and manage permissions.
SQL command divided into 5 main categories
- DDL (Data Definition Language): used to define the database structure, include: CREATE, ALTER, DROP, TRUNCATE
- DML (Data Manipulation Language): used to manipulated with the data stored in database, include: UPDATE, DELETE, INSERT
- DQL (Data Query Language): used to retrieve data from database, include: SELECT
- DCL (Data Control Language): use to manage user permission, access control, include: REVOKE, GRANT.
- TCL (Transaction Control Language): use to manage transaction, include: COMMIT, ROLLBACK, SAVEPOINT.
# 3. The order of SQL query
The logical execution order of a SQL query is : FROM, JOIN, WHERE, GROUP BY, HAVING, SELECT, DISTINCT, ORDER BY, LIMIT. Because the WHERE evaluated before SELECT, so it cannot reference a column alias defined in SELECT clause. Also, if LIMIT is used without ORDER BY, the database doesn’t guarantee the order of return rows.
# 4. Where and Having
Both all use for filtering, but different in the execution order
- Where evaluate before Group By –> it used to filter each rows. So it cannot use aggregate functions like COUNT( * ) or SUM ()
- Having evaluate after Group By –> use to filter each group.
So the rule is put all non aggregation condition in where clause for reducing the numbers of row.
# 5. Joins
The difference between Left, Right, Inner, Full Outer is the way they handle unmatched rows between two table. Inner Join return only matched rows. Left Join just keep all rows from the left table. Right Join just keep all rows from the right table. Full Outer return all rows from both, with missing value return it to null. Cross Join return cartesian product of two table, every rows from first table will combine with every row in second table.
[[SQL Example#1. JOINS]]
# 6. Null Handling
Null in SQL represent for unknow value, it not 0, empty or false. For checking Null, cannot use == or != because null is not considered equal any value, include another null. Instead, SQL provides IS NULL and IS NOT NULL operations.
# 7. Group By in SQL
GROUP BY is used to group rows have the same value in one or more columns into a single group. It is commonly use aggregate function like COUNT(), SUM(), MAX(), MIN(). Every non-aggregated function in SELECT must be appear in GROUP BY.
# 8. Index in SQL
Index is a database object used to improve speed of data’s retrieval. It similar like the index of a book. Instead of let the database scan full-table, by index it can quicky locate in the exact rows. However it have cost, for DML operations like INSERT, UPDATE, DELETE, database need more time to update the index.
# 9. Normalization
Normalization is a process of organizing data in multiple related data tables to reduce redundancy, improve data integrity. There are 3 common normalization
- 1NF: Removing repeating values make sure that every column contains atomic values.
- 2NF: Built on 1NF, removing partial dependencies all non-key value must be depend on entire primary key, matter when using composite key.
- 3NF: Built on 2NF, removing transitive dependencies, non-key columns cannot depend another non-key columns.
# 10. ORM, Advantages and Disadvantages
ORM (Object Relational Mapping) is a library or framework, uses for mapping the object in application to tables in database.
- Advantages: reducing the amount of SQL code, easy for change another DB, provide many built in features: caching, transaction manager
- Disadvantages: encounter N+1 query problem, the generated SQL may not be optimal.
# 11. N+1 Query
The N+1 query problem occur when an application first retrieve a list of parent entities with a single query, and then execute an additional query for each parent entity’s related data.
Solution: Retrieving both departments and employees in a single join query.


