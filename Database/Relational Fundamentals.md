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
# 2. SQL Fundamentals
## 2.1 DDL - Data Definition Language
DDL used to define, alter, or destroy the structure of your database objects (like tables, indexes, and databases). It doesn’t deal with individual rows of data
```MySQL
CREATE TABLE orders (
    id         BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id    BIGINT NOT NULL,
    total      DECIMAL(10, 2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

ALTER TABLE orders ADD COLUMN status VARCHAR(20) DEFAULT 'pending';
DROP TABLE orders;
TRUNCATE TABLE orders; -- removes all rows, keeps structure
```

## 2.2. DML - Data Manipulation Language 
DML is used to manage and modify the **data within** those structures. Once DDL has built the table, DML is how you add, change, or delete the actual records.
```SQL
INSERT INTO orders (user_id, total) VALUES (1, 99.90);

UPDATE orders SET status = 'shipped' WHERE id = 42;

DELETE FROM orders WHERE status = 'cancelled';
```

## 2.3. DQL – Data Query Language
**Retrieving** data from the database so you can look at it.
```SQL
SELECT u.name, COUNT(o.id) AS order_count, SUM(o.total) AS revenue
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.created_at >= '2024-01-01'
GROUP BY u.id, u.name
HAVING COUNT(o.id) > 5
ORDER BY revenue DESC
LIMIT 10;
```
# 3. Joins
| Join Type         | Returns                                                                                                                                                 |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `INNER JOIN`      | Rows matching in **both** tables                                                                                                                        |
| `LEFT JOIN`       | All rows from left + matching from right (NULL if no match)                                                                                             |
| `RIGHT JOIN`      | All rows from right + matching from left                                                                                                                |
| `FULL OUTER JOIN` | All rows from both tables                                                                                                                               |
| `CROSS JOIN`      | Cartesian product (every combination) don’t have `on` (It takes **every single row** from Table A and pairs it with **every single row** from Table B.) |
| `SELF JOIN`       | Table joined to itself (e.g. employee → manager)                                                                                                        |
# 4. Null Handling
NULL is not a value – it represents for unknow. Key rules:
- `NULL == NULL` –> false (use `IS NULL` / `IS NOT NULL`)
# 5. Constraints
|Constraint|Purpose|
|---|---|
|`NOT NULL`|Column must have a value|
|`UNIQUE`|No duplicate values (NULLs may be allowed)|
|`CHECK`|Custom validation expression|
|`DEFAULT`|Fallback value when none provided|
|`FOREIGN KEY`|Referential integrity|
# 6. Aggregate Functions
```SQL
SELECT
    COUNT(*)            AS total_rows,
    COUNT(DISTINCT uid) AS unique_users,
    SUM(amount)         AS total,
    AVG(amount)         AS average,
    MIN(amount)         AS minimum,
    MAX(amount)         AS maximum
FROM payments;
```
