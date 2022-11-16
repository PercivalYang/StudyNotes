Aggregate Function(聚合函数)，指执行一个或多个值的计算或其他操作，并返回**单个***值。
常见AF有：
- `SUM()`
- `MAX()`
- `MIN()`
- `AVG()`
- `COUNT()`
## `Group by` & `ORDER BY`
**Part 1**: 先来看一段代码
```sql
USE sql_invoicing;
SELECT
    date,
    pm.name,
    SUM(amount) AS total_payments
FROM payments p
JOIN payment_methods pm
	ON payment_method_id = payment_method
GROUP BY date,name   -- 1.
ORDER BY date   -- 2.
```
 - 上面代码要注意两点：
 1. `GROUP BY`后面的参数和SELECT里面选择的要一致，不能缺漏，否则会报错；
 2. `ORDER BY`必须在`GROUP BY`后面，放在前面会报错

**Part 2**: `GROUP BY`后面不能跟`WHERE`条件语句
- 解决方法：`HAVING`，例如：
```sql
SELECT 
    client_id,
    SUM(invoice_total) AS total_sales
FROM invoices
GROUP BY client_id
HAVING total_sales>500 -- 用HAVING代替WHERE
```