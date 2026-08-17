# 1. Aggregate after GROUP()
Use subquery
```sql
SELECT MAX(num) as num
FROM (
    SELECT num FROM MyNumbers
    GROUP BY num
    HAVING COUNT(*) = 1
)
```
# 2. Return the Null value
- Use JOIN 
- Use Aggregate Function
# 3. Return to 0 if empty row
- Use `COUNT()` 
- Use `COALESCE(..., 0)` : returns the first non-NULL value in a list of arguments.
# 4. Integer Division Truncation
When you divide integer to another integer is equals integer
```sql
SELECT 3 / 4; 
-- Result: 0   (Not 0.75! The .75 is dropped entirely)

SELECT ROUND(3 / 4, 2); 
-- Result: 0   (ROUND can't restore lost decimals)
```
Solve it by multiple with 0.1 (cast it to decimal)
```sql
SELECT 3 * 1.0 / 4;
-- Result: 0.750000

SELECT ROUND(3 * 1.0 / 4, 2);
-- Result: 0.75  (Correct!)
```
# 5. Calculate the Percentage
## 5.1. Percent of group for entire column
```sql
SELECT contest_id, ROUND(COUNT(DISTINCT user_id) * 100.0 / (SELECT COUNT(*) FROM Users), 2) as percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id ASC
```
# 6. Common Use Case of Self JOIN
## 6.1. Hierarchical Data
```sql
SELECT 
    e.employee_name,
    m.employee_name AS manager_name
FROM Employees e
LEFT JOIN Employees m 
    ON e.manager_id = m.employee_id;
```
## 6.2. Comparing Consecutive
```sql
SELECT 
    start_act.machine_id,
    ROUND(AVG(end_act.timestamp - start_act.timestamp), 3) AS processing_time
FROM Activity start_act
JOIN Activity end_act
    ON start_act.machine_id = end_act.machine_id
   AND start_act.process_id = end_act.process_id
WHERE start_act.activity_type = 'start'
  AND end_act.activity_type = 'end';
```
## 6.3. Comparing Rows Based on Dates
```sql
SELECT today.id
FROM Weather today
JOIN Weather yesterday
    ON DATEDIFF(today.recordDate, yesterday.recordDate) = 1
WHERE today.temperature > yesterday.temperature;
```
