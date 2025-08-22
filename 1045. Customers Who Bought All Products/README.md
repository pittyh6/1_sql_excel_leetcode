### Problem: 1045. Customers Who Bought All Products

<hr>

### Table

Input:
Customer table:

| customer_id    |   product_key   |
| :------------- | :-------------: |
| 1              |        5        |
| 2              |        6        |
| 3              |        5        |
| 3              |        6        |
| 1              |        6        |
| :------------- | :-------------: |

Product table:

|   product_key   |
| :-------------: |
|        5        |
|        6        |
| :-------------: |

<hr>

### SQL Solution

1. First solution

```sql
--RunTime 103ms
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(product_key) FROM Product)

```

<hr>

### Excel Solution
