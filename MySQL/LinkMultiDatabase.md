- `(INNER) JOIN`: `INNER` 可以不写
  - `INNER JOIN`: 默认为内连接，即数据库内部的连接
    - EX:
    ```sql
    -- product_id 要指明来自哪个DataBase
    SELECT oi.product_id, order_id
    FROM order_items oi    -- oi是前面变量名的自定义简写，下面的p同理
    JOIN products p 
      ON oi.product_id = p.product_id    -- 连接products数据库，并以product_id同步
    ```
   - `OUTER JOIN`: 外连接
     - `LEFT (OUTER) JOIN`: 当左表有不满足条件的items时，全部输出；
     - `RIGHT (OUTER) JOIN`：当右表有不满足条件的items时，全部输出；
     - EX：
       ```sql
        SELECT 
	      c.customer_id,
          c.first_name,
          o.order_id
        FROM customers c
        LEFT JOIN orders o
	      ON c.customer_id = o.customer_id
       ```
- 为避免ambiguous，多个数据库中的同一个表应加上前缀，前缀为该表所处数据库的名称。
- 自连接：
   - EX：
    ```sql
    USE sql_hr;
    SELECT *
    FROM employees e
    JOIN employees m    -- 自连接别名要不同
        ON e.reports_to = m.employees_id    -- reports_to表示管理员id，employees_id表示员工id，该表达式目的是找出每个员工负责的管理人员的信息
    ```
- UNION
  通过行（纵向拼接）来拼接表格，前面的JOIN是通过Column来拼接（即横向拼接）
```sql
USE sql_invoicing;
SELECT 
      'First half of 2019' AS date_range,
      SUM(invoice_total) AS total_sales,
      SUM(payment_total) AS total_payments,
      SUM(invoice_total-payment_total) AS what_we_expect
FROM invoices
WHERE due_date BETWEEN '2019-01-01' AND '2019-06-30'
UNION
SELECT
    'Second half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total-payment_total) AS what_we_expect
FROM invoices
WHERE due_date > '2019-06-30'
UNION
SELECT
    'Total' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total-payment_total) AS what_we_expect
FROM invoices
```