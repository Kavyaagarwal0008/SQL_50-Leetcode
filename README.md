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

## Q23- [Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT teacher_id , COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY teacher_id;
```

## Q24- [User Activity for the Past 30 Days 1](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE (activity_date > '2019-06-27' AND activity_date <= '2019-07-27')
GROUP BY activity_date;
```
## Q25 - [Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT s.product_id , s.year AS first_year, s.quantity, s.price
FROM Sales s

JOIN(
    SELECT product_id, MIN(year) AS year FROM Sales GROUP BY product_id

) p
ON s.product_id = p.product_id
AND s.year=p.year;
```

## Q26- [Classes with atleast 5 students](https://leetcode.com/problems/classes-with-at-least-5-students/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;
```
## Q27- [Find Followers Count](https://leetcode.com/problems/find-followers-count/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT user_id, COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id ASC;
```
## Q28- [Biggest Single Number](https://leetcode.com/problems/biggest-single-number/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT MAX(num) AS num
FROM MyNumbers
WHERE num IN (
    SELECT num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(num) = 1
);
```

## Q29 - [Customers Who Bought All Products]( https://leetcode.com/problems/customers-who-bought-all-products/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (
    SELECT COUNT(*) FROM Product
);
```
