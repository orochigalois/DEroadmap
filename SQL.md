# 2025-02-11 10:37:44 compare string length calculation functions in SQL, python and Javascript
reference: xxxxx

where LENGTH(string)>15
print(len(s))
console.log(s.length);
_______________________________________________________________
# 2025-02-12 17:03:25 what join is this one? SELECT * FROM Weather w1, Weather w2
reference: xxxxx

This is cross join, if Weather has 10 rows, then the result will be 10x10=100 rows
_______________________________________________________________
# 2025-02-12 17:37:41 sub query
reference: [197. Rising Temperature](https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=top-sql-50)

xxxxx
_______________________________________________________________
# 2025-02-23 11:46:27 what is union in sql?
keywords:
must same number of columns
data types should match

reference: xxxxx

❌ Incorrect (Mismatched Data Types)
SELECT id, salary FROM employees_a  -- id (INTEGER), salary (FLOAT)
UNION
SELECT emp_id, department FROM employees_b; -- emp_id (INTEGER), department (VARCHAR)

✅ Fix: Use CAST()
SELECT id, CAST(salary AS VARCHAR) FROM employees_a
UNION
SELECT emp_id, department FROM employees_b;

_______________________________________________________________
# 2025-02-25 15:54:44 difference between UNION and UNION ALL in SQL
keywords:
duplicate

reference: xxxxx

UNION: Removes duplicate rows
UNION ALL: Does not remove duplicates; all rows from the queries are included.
_______________________________________________________________
# 2025-02-27 10:11:32 difference between count(*) and count(id) in SQL
keywords:
null

reference: xxxxx

count(*): null will be counted
count(id): null will ***not*** be counted
_______________________________________________________________
# 2025-02-27 13:21:57 what is the join type for "table A JOIN table B" in SQL?
keywords:
inner join

reference: xxxxx

When you write a query like "table A JOIN table B" in SQL without explicitly specifying the type of join, it defaults to an INNER JOIN.
_______________________________________________________________
# 2025-02-28 21:38:14 how to use case in SQL?
keywords:
case end
when then
else

reference: xxxxx

SELECT 
    employee_name,
    salary,
    CASE 
        WHEN salary > 100000 THEN 'High Salary'
        WHEN salary BETWEEN 50000 AND 100000 THEN 'Medium Salary'
        ELSE 'Low Salary'
    END AS salary_category
FROM employees;

_______________________________________________________________
# 2025-02-28 21:39:13 what is the result?
keywords:
xxx
xxx
xxx

reference: xxxxx

select ifnull(2/0,0)
select 2/0
select 2/null
select sum(null)
_______________________________________________________________