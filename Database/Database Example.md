**Employees**

| id  | employee_name | department_id |
| --- | ------------- | ------------- |
| 101 | Alice         | 1             |
| 102 | Bob           | 2             |
| 103 | Charlie       | 1             |
| 104 | David         | NULL          |
| 105 | Eva           | 5             |
**Department**

| id | department_name |
| -- | --------------- |
| 1  | Engineering     |
| 2  | Human Resources |
| 3  | Marketing       |
| 4  | Finance         |

# 1. JOINS
## 1.1. Inner Join
![[Pasted image 20260708180057.png]]
## 1.2. Left Join
![[Pasted image 20260708180156.png]]
## 1.3. Right Join
![[Pasted image 20260708180229.png]]
## 1.4. Full Outer Join
![[Pasted image 20260708180409.png]]
## 1.5. Cross Join
![[Pasted image 20260708180825.png]]

# 2. Normalization
## 2.1. 1NF
|StudentID|Name|Courses|
|---|---|---|
|1|Phuc|Java, SQL, Docker|
|2|An|Java, Python|
## 2.2. 2NF
| StudentID | CourseID | StudentName | CourseName | Grade |
| --------- | -------- | ----------- | ---------- | ----- |
| 1         | 101      | Phuc        | Java       | A     |
| 1         | 102      | Phuc        | SQL        | B     |
| 2         | 101      | An          | Java       | A     |
The primary key is `(StudentID + CourseID)` but `StudentName` depends only on `StudentID`, not the whole `(StudentID, CourseID)`, `CourseName` depends only on `CourseID`.
## 2.3. 3NF
| StudentID | StudentName | DepartmentID | DepartmentName   |
| --------- | ----------- | ------------ | ---------------- |
| 1         | Phuc        | 10           | Computer Science |
| 2         | An          | 20           | Mathematics      |
| 3         | Minh        | 10           | Computer Science |
`DepartmentName` doesn't directly depend on `StudentID`. It depends on `DepartmentID`.
# 3. Core DML Statement
## 3.1. Insert
```sql
INSERT INTO Salary (id, name, sex, salary)
VALUES (1, 'Alex', 'm', 5000);
```
## 3.2. Update
```sql
UPDATE Salary
SET salary = 6000
WHERE id = 1;
```
## 3.3. Delete
```sql
DELETE FROM Salary
WHERE id = 1;
```
# 4. N+1
```java
// You fetch all departments:
List<Department> departments = departmentRepository.findAll();
// Hibernate Executes
SELECT * FROM department;
// Suppose there are 3 departments
// Then you do
for (Department department : departments) {
    System.out.println(department.getEmployees());
}
// Hibernate execute
SELECT * FROM employee WHERE department_id = 1;
SELECT * FROM employee WHERE department_id = 2;
SELECT * FROM employee WHERE department_id = 3;
// So: 1 query + N Query
```
