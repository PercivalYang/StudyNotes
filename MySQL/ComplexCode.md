## Subqueries
代码同样可以嵌套使用，即在主查询代码中还包含一个字查询代码，例如：
```sql
SELECT 
    e.employee_id,
    e.first_name
FROM employees e
WHERE e.salary>(
    SELECT AVG(salary)   -- 子查询
    FROM employees
)
```