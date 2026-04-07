# SQL_50-Leetcode
50 SQL questions from leetcode solution

# SELECT
## Q1 - [Recyclable and Low Fat Products ](https://leetcode.com/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT product_id 
FROM Products
WHERE low_fats='Y' AND recyclable='Y';
```
## Q2 - [Find Customer Referee](https://leetcode.com/problems/find-customer-referee/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT name FROM Customer
WHERE referee_id<>2 OR referee_id IS NULL;
```
## Q3 - [Big Countries](https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT name,population,area 
FROM World 
WHERE area>=3000000 
OR population >= 25000000;
```
## Q4 - [Article Views I](https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT DISTINCT author_id as id FROM Views 
WHERE author_id=viewer_id 
ORDER BY author_id;
```
## Q5 - [Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT tweet_id FROM Tweets
WHERE LENGTH(content)>15;
```

# Basic Joins

## Q6 - [Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT eu.unique_id,e.name
FROM EmployeeUNI eu
RIGHT JOIN Employees e
ON e.id=eu.id
```
## Q7 - [Product Sale Analysis I](https://leetcode.com/problems/product-sales-analysis-i/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT p.product_name, s.year, s.price
FROM Sales s
INNER JOIN Product p
ON s.product_id=p.product_id;
```
## Q8 - [Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT v.customer_id, COUNT(*) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t
ON v.visit_id=t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id;
```
## Q9 - [Rising Temperature](https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT w1.id
FROM Weather w1
JOIN Weather w2
ON w1.recordDate = w2.recordDate + INTERVAL '1 day'
WHERE w1.temperature > w2.temperature;
```
## Q10 - [Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT a1.machine_id,
       ROUND(AVG((a2.timestamp - a1.timestamp)::numeric), 3) AS processing_time
FROM Activity a1
JOIN Activity a2
ON a1.machine_id = a2.machine_id
AND a1.process_id = a2.process_id
WHERE a1.activity_type = 'start'
  AND a2.activity_type = 'end'
GROUP BY a1.machine_id;

```
## Q11 - [Employee Bonus](https://leetcode.com/problems/employee-bonus/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT e.name,b.bonus
FROM Employee e
LEFT JOIN Bonus b
ON e.empId=b.empId
WHERE b.bonus IS NULL OR b.bonus<1000;
```
## Q12 - [Students and Examinations](https://leetcode.com/problems/students-and-examinations/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT St.student_id,St.student_name,s.subject_name, COUNT(e.subject_name) AS attended_exams
FROM Students St
CROSS JOIN Subjects s
LEFT JOIN Examinations e

ON St.student_id = e.student_id
AND s.subject_name = e.subject_name
GROUP BY St.student_id,St.student_name,s.subject_name
ORDER BY St.student_id, s.subject_name;
```
## Q13 - [Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT name
FROM Employee
WHERE id IN (
    SELECT e1.managerId
    FROM Employee e1
    LEFT JOIN Employee e2
    ON e1.managerId = e2.id
    GROUP BY e1.managerId
    HAVING COUNT(e1.managerId) >= 5
);
```
## Q14 - [Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT s.user_id, 
    ROUND(
        IFNULL(
            SUM(c.action='confirmed')/COUNT(c.action),0
        ),
        2
    ) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c
On s.user_id=c.user_id
GROUP BY s.user_id;

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

# Advanced Select and Joins

## Q30 - [The Number of Employess Which Report to Each Employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT e.employee_id,e.name,
COUNT(r.employee_id) AS reports_count,
ROUND(AVG(r.age)) AS average_age

FROM Employees e
JOIN Employees r
ON e.employee_id=r.reports_to
GROUP BY e.employee_id,e.name
ORDER BY e.employee_id;
```
## Q31 - [Primary Department for Each Employee](https://leetcode.com/problems/primary-department-for-each-employee/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT employee_id, department_id FROM Employee WHERE employee_id IN (
SELECT employee_id FROM Employee
GROUP BY employee_id HAVING COUNT(*) =1) OR primary_flag = 'Y'
```
## Q32 - [Triangle Judgement](https://leetcode.com/problems/triangle-judgement/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT *,
    CASE 
        WHEN GREATEST(x, y, z) < (x + y + z - GREATEST(x, y, z)) THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM Triangle;
```
## Q33 - [Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/description/?envType=study-plan-v2&envId=top-sql-50)
```sql

```
## Q34 - []()
```sql

```
## Q35 - []()
```sql

```
## Q36 - []()
```sql

```
## Q37 - [Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company/?envType=study-plan-v2&envId=top-sql-50)
```sql

```
## Q38 - [Exchange Seats]()
```sql

```
## Q39 - [Movie Rating]()
```sql

```
## Q40 - [Restaurant Growth]()
```sql

```
## Q41 - [Friend Requests II: Who Has the Most Friends]()
```sql

```
## Q42 - [Investments in 2016](https://leetcode.com/problems/investments-in-2016/?envType=study-plan-v2&envId=top-sql-50)
```sql

```
## Q43 - [Department Top Three Salaries](https://leetcode.com/problems/employees-whose-manager-left-the-company/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000
AND manager_id NOT IN (
    SELECT employee_id FROM Employees
)
ORDER BY employee_id;
```
## Q44 - [Fix Names in a Table]()
```sql

```
## Q45 - [Patients With a Condittion]()
```sql

```
## Q46 - [Delete Duplicate Emails]()
```sql

```
## Q47 - [Second Highest Salary]()
```sql

```
## Q48 - [Group Sold Products By the Date]()
```sql

```
## Q49 - [List the Products Ordered in a Period]()
```sql

```
## Q50 - [Find Users with Valid E-Mails]()
```sql

```

