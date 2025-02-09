### 2025-02-09 13:05:30 this is more like a think of this as a scenario like a you know what if um so let's say you have a ticket which comes to you and it says on our dashboard there's duplicate customers so how would you responds to that ticket as a data engineer
reference: xxxxx

- start by checking the SQL queries or logic behind the dashboard to see how the customer data is being aggregated or filtered
- copy this query into bigquery and debug this query
- if the query is correct, then we can check the fact table, there are 2 possibilities here:
- data source is wrong, have duplicate row, need to communicate with the other squad, let them know
- ETL pipeline has a bug
_______________________________________________________________