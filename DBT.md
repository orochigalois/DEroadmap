https://www.youtube.com/watch?v=G3t0EF6hMkQ

https://www.youtube.com/watch?v=DWHdT2qQgxI

https://www.youtube.com/watch?v=5mDutGU24MU&t=22s

https://www.youtube.com/watch?v=T2CS9a63LOM

### 2025-02-07 13:35:11 how to use dbt package dbt-external-tables to ingest a big csv to big query
reference: xxxxx

xxxxx
_______________________________________________________________
### 2025-02-07 21:20:36 how to use `bq load` ingest a 2G csv into bigquery
reference: xxxxx

![alt text](image-2.png)
xxxxx
_______________________________________________________________
### 2025-02-07 21:25:21 What is a DBT Model?
reference: [https://youtu.be/G3t0EF6hMkQ?t=83](https://youtu.be/G3t0EF6hMkQ?t=83)

A DBT model is a .sql file that contains SELECT statements, WITH clauses, or Common Table Expressions (CTEs). When you run a DBT model, a table or view is created based on the materialization property.
_______________________________________________________________
### 2025-02-07 21:29:28 what is dbt macro
reference: xxxxx

 a macro is a reusable piece of SQL or Jinja code that can be called within dbt models, tests, analyses, or other macros
_______________________________________________________________
### 2025-02-08 21:44:16 what will the following incremental modal create in big query?
![alt text](image-3.png)
reference: xxxxx

by default, dbt uses Append-Only strategy, New records are inserted without checking for duplicates.   
before:  
![alt text](image-4.png) 
after dbt run:  
![alt text](image-5.png)
_______________________________________________________________
### 2025-02-08 21:55:11 do you use any dbt packages by any chance?
reference: xxxxx

dbt_utils – A must-have package that provides utility macros for common transformations like safe casting, generating surrogate keys, pivoting, and unpivoting.

dbt-unit-testing - allows writing unit tests in YAML with mocked inputs and expected outputs.
_______________________________________________________________
