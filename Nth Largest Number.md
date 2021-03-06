# 176. Second Highest Salary

## Question Description

Write a SQL query to get the second highest salary from the Employee table.


| Id | Salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

For example, given the above Employee table, the query should return 200 as the second highest salary. If there is no second highest salary, then the query should return null.


| SecondHighestSalary |
|---------------------|
| 200                 |


## Solution 1

Compare the (same) columns from two (same) tables

Get the max, distinct value that are less than at least one value in other (same) column

KEY: add max before distinct, or will not show "NULL" but empty instead

```mysql
SELECT MAX(distinct e.Salary)  AS SecondHighestSalary
FROM Employee AS e, Employee AS ec
WHERE e.Salary <ec.Salary
```

## Solution 2

KEY: use IFNULL to show "NULL" instead of empty

IFNULL(expression, alt_value)

KEY: LIMIT n OFFSET m 

```mysql
SELECT
    IFNULL(
      (SELECT DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
        LIMIT 1 OFFSET 1),
    NULL) AS SecondHighestSalary
```

# 177. Nth Highest Salary

## Question Description

Write a SQL query to get the nth highest salary from the Employee table.

| Id | Salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.

| getNthHighestSalary(2) |
|------------------------|
| 200                    |


## Solution 1

KEY: FUNCTION

Compare (same) columns from (same) columns

KEY: use DISTINCT in (1) SELECT (2) COUNT 

```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      SELECT DISTINCT e.Salary  AS NthHighestSalary
      FROM Employee AS e, Employee AS ec
      WHERE e.Salary <= ec.Salary
      GROUP BY e.Id
      HAVING COUNT(DISTINCT ec.Salary ) = N
      
  );
END

```

## Solution 2

KEY: DECLARE, SET

KEY: LIMIT n, m: skip n rows and keep m rows

```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
DECLARE M INT;
SET M=N-1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT Salary 
      FROM Employee 
      ORDER BY Salary DESC 
      LIMIT M, 1
  );
END
```

