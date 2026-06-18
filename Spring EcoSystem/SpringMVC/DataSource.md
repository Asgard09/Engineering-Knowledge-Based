# 1. Definition
Data Source is an abstraction that provide database connections. Instead of creating a new connection every time data access is required, the application will request data source
# 2. Why it matters
## 2.1. The world before
Every time needing data access:
- Load JDBC Driver
- Create the connection
- Execute the query
- Close connection
The problem: 
- The connection is expensive: establish TCP hand sake, Authentication User, Access server Resource, Create Session State (with many request –> huge overhead).
- Boiler plate code: need to define the same data access code many time
- Tightly Coupled on Specific Driver: want to switch Postgres –> change entire code
## 2.2. How it solved well?
When the request need database access –> they request the connection from Data Source. Modern Spring Boot application use a Data Source  implemented by HikariCP provides connection pooling with default pool size is 10, every time application needs database access, it will borrow the connection in the pool, after completed return it to pool, if the connection pool is empty –> req will be waiting

