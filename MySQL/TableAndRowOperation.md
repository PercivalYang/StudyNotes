# Table
## Create table by copy
- Ex:
```sql
CREATE TABLE invoices_archived AS   -- 创建表格
-- 创建表格的数据从下方进行复制
SELECT 
    i.invoice_id,
    i.number,
    c.name,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.due_date,
    i.payment_date
FROM invoices i
JOIN clients c
	ON c.client_id = i.client_id
WHERE i.payment_date IS NOT NULL
```
## Row
### Column property
![](/i/fde3ac27-ca3d-4cda-a903-084ce45c2e6d.jpg)
解释上图几个关键字：
- `VARCHAR(50)`: 是个长度变化的字符串数据(variable char)类型，和`CHAR(50)`的区别是如果输入的字符串长度为5(即小于50)，CHAR会在字符串后填充空格以达到50的长度，而VARCHAR不会，达到节省空间的目的；
- `PK`: Primary Key，等同于代表表格内每行的ID；
- `NN`: Not Null, 即刚Column不能是NULL；
- `AI`: Auto Increment，若输入为DEFAULT，数字大小会递增
- `Default / Expression`: 若输入为DEFAULT时的默认表达式
### Insert
```sql
INSERT INTO products (
    product_id,  -- products
    name,
    quantity_in_stock,
    unit_price)
VALUES (DEFAULT,'n1',50,1),
	   (DEFAULT,'n2',40,2)
```
### Update Row
```sql
UPDATE invoices
SET 
  payment_total = invoice_total,  -- 可以等于对应的某列
  payment_date = NULL,  -- 可以等于某个值
WHERE invoice_id = 1 -- 在满足condition的Row更新上述Column