# 1. Aggregate after GROUP()
Use subquery
```sql
SELECT MAX(num) as num
FROM MyNumbers
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
