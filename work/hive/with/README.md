WITH语句用于创建命名的子查询或公共表达式，以便在后续查询中引用。这有助于简化复杂查询和提高代码重用性，
示例：
```
WITH temp_table AS (
  SELECT customer_id, SUM(order_total) AS total_spent
  FROM orders
  GROUP BY customer_id
)
SELECT *
FROM temp_table
WHERE total_spent > 1000;
```