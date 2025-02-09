### 2025-02-09 13:05:30 this is more like a think of this as a scenario like a you know what if um so let's say you have a ticket which comes to you and it says on our dashboard there's duplicate customers so how would you responds to that ticket as a data engineer
reference: xxxxx

- start by checking the SQL queries or logic behind the dashboard to see how the customer data is being aggregated or filtered
- copy this query into bigquery and debug this query
- if the query is correct, then we can check the fact table, there are 2 possibilities here:
- data source is wrong, have duplicate row, need to communicate with the other squad, let them know
- ETL pipeline has a bug
_______________________________________________________________
### 2025-02-09 14:37:39 are there any aspects of DBT which might be able to help you in looking this issue

reference: xxxxx

- Built-in Tests: DBT offers built-in tests that can help you identify duplicates. You can use unique tests to ensure that a specific column, like customer\_id, is unique across the dataset. If the test fails, it's a clear indication that duplicates exist.

- Custom Tests: You can create custom tests in DBT to check for specific conditions that might lead to duplicates. For example, you can write a test to check for rows where a combination of fields (like customer\_name and email) should be unique but isn't.

_______________________________________________________________