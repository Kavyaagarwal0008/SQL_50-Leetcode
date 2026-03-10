# SQL_50-Leetcode
50 SQL questions from leetcode solution

# SELECT
## Q1 - [Recyclable and Low Fat Products ](https://leetcode.com/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT product_id 
FROM Products
WHERE low_fats='Y' AND recyclable='Y';
```

# Sorting and Grouping
## Q - [Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT s.product_id , s.year AS first_year, s.quantity, s.price
FROM Sales s

JOIN(
    SELECT product_id, MIN(year) AS year FROM Sales GROUP BY product_id

) p
ON s.product_id = p.product_id
AND s.year=p.year;
```

## Q - [Customers Who Bought All Products]( https://leetcode.com/problems/customers-who-bought-all-products/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (
    SELECT COUNT(*) FROM Product
);
```
