### Table
Employee


| Column Name | Type    |
| :-----------|:--------|
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |

<hr>

### Problem: 570. Managers with at Least 5 Direct Reports
id is the primary key (column with unique values) for this table.
Each row of this table indicates the name of an employee, their department, and the id of their manager.
If managerId is null, then the employee does not have a manager.
No employee will be the manager of themself.
 

Write a solution to find managers with at least five direct reports.

Return the result table in any order.

<hr>

### SQL Solution
1) First solution
```sql
WITH count_managers AS(
    SELECT managerId, COUNT(managerId) AS count_manager, name
    FROM Employee
    GROUP BY managerId
)
SELECT e.name
FROM Employee e
INNER JOIN count_managers cm
    ON cm.managerId = e.id
WHERE count_manager >= 5;
```
2) Second Solution
```sql
SELECT e.name
FROM Employee e
LEFT JOIN Employee ec
    ON ec.managerId = e.id
GROUP BY ec.managerId
HAVING COUNT(ec.managerId) >= 5;
```

<hr>

### Excel Solution
